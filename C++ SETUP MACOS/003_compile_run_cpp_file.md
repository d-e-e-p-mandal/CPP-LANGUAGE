# HOW TO COMPILE & RUN CPP FILE :

## Compile :
### Type 1 :
`clang++ --std=c++17 fileName.cpp ` : Generally : It generate macos: a.out or windows : a.exe exeutable file

`clang++ --std=c++17 abc.cpp -o fileName ` : it generates fileName executable file

### Type 2 :

`g++ -std=c++17 fileName.cpp` : it generate macos: a.out or windows : a.exe exeutable file
`g++ -std=c++17 fileName.cpp -o fileName` : it generates fileName executable file

#### `Note : -o fileName (anyName) (We can use any name : fileName is recomended) `


## run : `./filename`

---
### Example :
File Saved as : abc.cpp

Compile :
`g++ -std=c++17 abc.cpp` or `clang++ --std=c++17 abc.cpp `

Run : Macos : ./a.out Window : ./a.exe (It may be change)


Compile :
`clang++ --std=c++17 abc.cpp -o abc ` or
`g++ -std=c++17 abc.cpp -o abc`

**RUN :** `./abc`