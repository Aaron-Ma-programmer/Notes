# Makefile & CMake

## 一、Makefile

### Version1：

> ```cpp
> # hello为生成的可执行文件，依赖于后面的三个.cpp文件
> # g++前面加一个TAB的空格
> hello: main.cpp printhello.cpp factorial.cpp
> 	g++ -o hello main.cpp printhello.cpp factorial.cpp
> ```

>  修改某个.cpp文件，再执行命令make，即可刷新执行文件hello
>
> 缺点：文件多时，编译时间长

### Version2：

> ```cpp
> CXX = g++
> TARGET = hello
> OBJ = main.o printhello.o factorial.o
> # make时执行g++ 先找TARGET，TARGET不存在找OBJ，OBJ不存在，编译三个.cpp文件生成.o文件
> # 然后再编译OBJ文件，生成可执行文件hello
> $(TARGET): $(OBJ)
> 	$(CXX) -o $(TARGET) $(OBJ)
> # main.o这样来的，编译main.cpp生成
> main.o: main.cpp
> 	$(CXX) -c main.cpp
> printhello.o: printhello.cpp
> 	$(CXX) -c printhello.cpp
> factorial.o: factorial.cpp
> 	$(CXX) -c factorial.cpp
> ```
>
> 优点：编译时间缩短。
>
> 缺点：每一个.cpp文件都需要写一个规则生成.o文件

### Version3：

> ```cpp
> CXX = g++
> TARGET = hello
> OBJ = main.o printhello.o factorial.o
>  
> # 编译选项，显示所有的warning
> CXXLAGS = -c -Wall
>  
> # $@表示的就是冒号前面的TARGET，$^表示的是冒号后OBJ的全部.o依赖文件
> $(TARGET): $(OBJ)
> 	$(CXX) -o $@ $^
>  
> # $<表示指向%.cpp依赖的第一个，但是这里依赖只有一个
> # $@表示指向%.o
> %.o: %.cpp
> 	$(CXX) $(CXXLAGS) $< -o $@
>  
> # 为了防止文件夹中存在一个文件叫clean
> .PHONY: clean
>  
> # -f表示强制删除，此处表示删除所有的.o文件和TARGET文件
> clean:
> 	rm -f *.o $(TARGET)
> ```
>
> 优点：统一规则，不需要每个规则单独列出。
>
> 缺点：每新增一个依赖都需要在OBJ中列出来。

### Version4:

> ```cpp
> # VERSION 4
> CXX = g++
> TARGET = hello
> # 所有当前目录的.cpp文件都放在SRC里面
> SRC = $(wildcard *.cpp)
> # 把SRC里面的.cpp文件替换为.o文件
> OBJ = $(patsubst %.cpp, %.o,$(SRC))
>  
> CXXLAGS = -c -Wall
>  
> $(TARGET): $(OBJ)
> 	$(CXX) -o $@ $^
>  
> %.o: %.cpp
> 	$(CXX) $(CXXLAGS) $< -o $@
>  
> .PHONY: clean
> clean:
> 	rm -f *.o $(TARGET)
> ```
>
> 优点：不需要手动列出每一个依赖，程序自动检索所有.cpp文件并替换成.o文件。

## 二、CMake

>```cpp
>camke_minimum_required(VERSION 3.10)
>    
>project(hello)
>    
>add_executable(hello main.cpp factorial.cpp printhello.cpp)
>```

> ```cpp
> //为了方便对cmake新生成文件进行删除，我们统一将生成文件放置一个文件夹中。
> mkdir build
> cd build
> cmake ..
> .\hello
> //	rm -rf build/
> ```
>
> cmake可以帮助我们生成所需makefile，并且自动检索我们的编译器、编译选项等。提升程序跨平台性。

## 三、Makefile & CMake 对比

### 1、Makefile：

- **语法**：Makefile 使用一种特定的语法来定义构建规则和依赖关系。
- **跨平台**：Makefile 本身不是跨平台的。不同的操作系统可能需要不同的 Makefile。
- **工具链**：Makefile 通常与 Unix-like 系统中的 `make` 工具一起使用。
- **灵活性**：Makefile 非常灵活，可以手动编写复杂的构建规则。
- **学习曲线**：Makefile 的语法可能对初学者来说比较复杂。

### 2、CMake：

- **语法**：CMake 使用更简单的语法来定义构建规则和依赖关系。
- **跨平台**：CMake 是跨平台的。它可以生成不同平台（如 Linux、Windows、macOS）的原生构建文件。
- **工具链**：CMake 可以生成多种构建系统的文件，如 Makefile、Ninja、Visual Studio 项目文件等。
- **灵活性**：CMake 通过提供宏和函数来简化构建过程，但可能不如 Makefile 灵活。
- **学习曲线**：CMake 的语法相对简单，易于上手。

总的来说，Makefile 更适合于需要精细控制构建过程的项目，而 CMake 由于其跨平台能力和易用性，更适合于大型或需要在多个平台上构建的项目。