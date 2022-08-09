---
layout: post
title:  "Python и работа в thread"
date: 2022-08-08 11:22:00 +0500
categories: jekyll update
---

Интерактивное консольное приложение на python.

В коде надо задать OID, которые надо считывать.

На компьютере должен быть подключен COM с подключенным GNSS-модулем.

Периодически считываем данные с девайса и пишем одновременно положение компьютера с GNSS - модуля в csv-файл.

Для меня здесь было из нового, что надо делать полезную работу и одновременно отвечать на запросы пользователя( Остановить работу по Ctrl+C).


А вот так ещё можно сделать исполняемый файл, для винды и unix.

```
wine pip install -r requirements.txt
wine pyinstaller -F -c src/main.py

pip install -r requirements.txt
pip install pyinstaller
pyinstaller -n main_unix -F -c src/main.py
```
Только надо не забыть, что Glibc должны на сборочной машине быть не новее, чем так где будем запускать.


```python
#!/usr/bin/env python

import argparse
import contextlib
import signal
import sys
from datetime import datetime
from threading import Event, Thread
from time import sleep

import serial
from pysnmp.error import PySnmpError
# from easysnmp import Session
# from easysnmp.exceptions import EasySNMPError
from pysnmp.hlapi import *
from pyubx2 import UBXReader

last_gps_value = ["NOT_GPS_LAT", "NOT_GPS_LON"]


def get_last_gps_value(com, task_done):
    global last_gps_value
    if com:
        try:
            stream = serial.Serial(com, 9600, timeout=3)
        except serial.serialutil.SerialException:
            return
        ubr = UBXReader(stream)
        for (_, parsed_data) in ubr.iterate():
            if task_done.is_set():
                return
            if parsed_data.identity == "GNGGA":
                last_gps_value = [str(parsed_data.lat), str(parsed_data.lon)]


class SNMPWalkException(Exception):
    pass


def walk(host, oid, timeout):
    out = []
    for (errorIndication, errorStatus, errorIndex, varBinds) in nextCmd(
        SnmpEngine(),
        CommunityData("public"),
        UdpTransportTarget((host, 161), timeout=timeout, retries=0),
        ContextData(),
        ObjectType(ObjectIdentity(oid)),
        lookupMib=False,
        lexicographicMode=False,
    ):
        if errorIndication:
            print(errorIndication, file=sys.stderr)
            raise SNMPWalkException
        if errorStatus:
            print(
                "%s at %s"
                % (
                    errorStatus.prettyPrint(),
                    errorIndex and varBinds[int(errorIndex) - 1][0] or "?",
                ),
                file=sys.stderr,
            )
            raise SNMPWalkException
        for varBind in varBinds:
            out.append((varBind[0].prettyPrint(), varBind[1].prettyPrint()))
    return out


@contextlib.contextmanager
def smart_open(filename=None):
    if filename and filename != "-":
        fh = open(filename, "w")
    else:
        fh = sys.stdout
    try:
        yield fh
    finally:
        if fh is not sys.stdout:
            fh.close()


def get_elem_from_walk(neighbor_stat, num):
    return [x for x in neighbor_stat if x[0].startswith(f"{OID_ALL_ELEMENTS}.{num}.")]


def name_stat_file(ip):
    return "{0}-{1}.csv".format(datetime.now().strftime("%Y_%m_%d_%H_%M"), ip)


OID_ALL_ELEMENTS = "1.3.6.1.4.1.3942.1.1.1.2.1"
OID_ELEM_HEX = {"SomeValueHex": 2}

OID_ELEM_STR = {"SomeStrValue": 3}
OID_ELEM_INT = {"SomeIntValue": 4}

GPS_VALUES = ("LATITUDE", "LONGTITUDE")


def monitor(ip, task_done, timeout):
    global last_gps_value
    name_file_csv = name_stat_file(ip)
    print(f"File for stat: {name_file_csv}")
    # session = Session(hostname=ip, community="public", version=2)
    try:
        walk(ip, ".1.3.6.1.4.1.3942.1.1.5.1.1.1.1.4", timeout)
    except SNMPWalkException:
        print(
            f"Device {ip} not UP or snmp not worked.\n" "Please run 'snmpd start'",
            file=sys.stderr,
        )
    with open(name_file_csv, "w", encoding="utf-8") as f:
        head = (
            ["DATETIME"]
            + list(GPS_VALUES)
            + list(OID_ELEM_STR.keys())
            + list(OID_ELEM_INT.keys())
        )
        print(",".join(head), file=f)
        while True:
            f.flush()
            if task_done.is_set():
                return
            sleep(timeout)
            date = str(datetime.now())
            try:
                neighbours_raw_stat = walk(ip, OID_ALL_ELEMENTS, timeout)
            except SNMPWalkException:
                print(f"{date} {ip}| walk failed, continue")
                print(
                    ",".join(
                        [date]
                        + last_gps_value
                        + ["DEVICE_NOT_UP"]
                        + ["-"] * (len(OID_ELEM_INT))
                    ),
                    file=f,
                )
                continue
            print(f"{date} {ip}| got walk")
            neighbours_stat = []
            for elem in get_elem_from_walk(neighbours_raw_stat, "1"):
                neighbours_stat.append([])
            if not neighbours_stat:
                neighbours_stat.append([])
                print(
                    ",".join(
                        [date]
                        + last_gps_value
                        + ["NO_VALUES"]
                        + ["-"] * (len(OID_ELEM_INT))
                    ),
                    file=f,
                )
                continue

            for line in neighbours_stat:
                print(
                    ",".join([date] + last_gps_value + line),
                    file=f,
                )
                f.flush()


def main():
    parser = argparse.ArgumentParser(description="probe")
    parser.add_argument(
        "--timeout", type=int, help="Timeout for requests in milliseconds", default=500
    )
    parser.add_argument(
        "--com", type=str, help="Com-port for GNSS-module", default="COM3"
    )
    parser.add_argument(
        "ip_address", metavar="IP", type=str, nargs="*", help="IP-address devices"
    )
    args = parser.parse_args()
    ip_dev = args.ip_address
    timeout = float(args.timeout) / 1000
    if not ip_dev:
        print("Need set ip_address for monitor values", file=sys.stderr)
        parser.print_help()
        sys.exit(2)
    print(ip_dev)

    task_done = Event()
    threads = []
    gps_thread = Thread(target=get_last_gps_value, args=(args.com, task_done))
    threads.append(gps_thread)
    gps_thread.start()

    for ip in ip_dev:
        t = Thread(target=monitor, args=(ip, task_done, timeout))
        threads.append(t)
        t.start()

    def signal_handler(sig, frame):
        print("You pressed Ctrl+C!")
        print("Save data")
        task_done.set()

    signal.signal(signal.SIGINT, signal_handler)
    while not task_done.is_set():
        sleep(1)

    for thr in threads:
        thr.join()
    return True


if __name__ == "__main__":
    sys.exit(main())
```

<!-- :public: -->
