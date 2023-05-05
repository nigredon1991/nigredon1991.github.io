
layout: post
title:  "Something"
date: 2023-05-04 18:14:00 +0500
categories: jekyll update

# Python Typing

Настройка nvim для lsp:
```lua
require("lspconfig").pyright.setup({
  | before_init = function(_, config)
  | ¦ ┆ config.settings.python.pythonPath = get_python_path(config.root_dir)
  | end,
  | on_attach = on_attach,
  | flags = lsp_flags
})
```

## Примеры кода

### Опциональные входные переменные
``` python
def print_hello(name: str | None=None) -> None:
    print(f"hello, {name}" if name is not None else "hello anon!")
```

### Typed Dict
```python
# Здесь мы можем считывать историю из файла, и записывать её обратно. Опущены функции, обработки
class History(TypedDict):
   date: str
def read(file) -> list[History]:              
   with open(file, "r") as f:                     
      return json.load(f)                                  
                                                            
def write(file, history: list[History]) -> None:      
   with open(file, "w") as f:
      json.dump(history, f, ensure_ascii=False, indent=4)
```

### Protocol 
```python
# Здесь у нас может пример протокола, как мы можем сохранять историю
class Storage(Protocol):                           
    """Interface for any storage saving History"""        
    |                                                     
    def save(self, history: History) -> None:             
    |   raise NotImplementedError                         
                                                          
                                                          
class PlainFileHistoryStorage:                            
    """Store history in plain text file"""                
    |                                                     
    def __init__(self, file: Path):                       
    |   self._file = file                                 
    |                                                     
    def save(self, history: History) -> None:             
    |   now = datetime.now()                              
    |   formatted_weather = format_history(history)       
    |   with open(self._file, "a") as f:                  
    |   ¦   f.write(f"{now}\n{formatted_history}\n")      

def save_history(history: History, storage: HistoryStorage) -> None: 
    """Saves history in the storage"""                               
    storage.save(history)                                            
```

### dataclasses and Enums
```python
from dataclasses import dataclass
from enum import Enum
from typing import Literal, TypeAlias

TypeInt: TypeAlias = int

# Здесь мы можем обращаться к Enum, как к строкам
# Вот так
# StringType.Lala1
class StringType(str, Enum):
    Lala1 = "1"
    Lala2 = "2" 
    Lala3 = "3"       
    |                    
    def __str__(self):   
    |   return self.value


@dataclass(slots=True, frozen=True)
class History:
    Len: TypeInt
    type: Stringtype
    sunrise: datetime

# Literal позволяет указать конкретные строки, которые могу придти в вводе
def _parse_history_time(o_dict: dict, time: Literal["string1"] | Literal["string2"]
) -> datetime:
    return datetime.fromtimestamp(o_dict["sys"][time])

```


<!-- :public: -->
#python