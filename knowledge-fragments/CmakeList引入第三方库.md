# CmakeList.txt引入第三方库

## DLL和LIB区别
- (1)lib是编译时用到的，dll是运行时用到的。如果要完成源代码的编译，只需要 lib；如果要使动态链接的程序运行起来，只需要dll。
- (2)如果有dll文件，那么lib一般是一些索引信息，记录了dll中函数的入口和位 置，dll中是函数的具体内容；如果只有lib文件，那么这个lib文件是静态编译出来的，索引和实现都在其中。使用静态编译的lib文件，在运行程序时 不需要再挂动态库，缺点是导致应用程序比较大，而且失去了动态库的灵活性，发布新版本时要发布新的应用程序才行。
- (3)动态链接的情况下，有两个 文件：一个是LIB文件，一个是DLL文件。LIB包含被DLL导出的函数名称和位置，DLL包含实际的函数和数据，应用程序使用LIB文件链接到DLL 文件。在应用程序的可执行文件中，存放的不是被调用的函数代码，而是DLL中相应函数代码的地址，从而节省了内存资源。DLL和LIB文件必须随应用程序 一起发行，否则应用程序会产生错误。如果不想用lib文件或者没有lib文件，可以用WIN32 API函数LoadLibrary、GetProcAddress装载。

## CmakeList.txt
### 以引入opencv为例
````cmake
// 添加第三方库头文件
include_directories(/home/opencv-3.0/include)
// 添加第三方库链接
link_directories(/home/opencv-3.0/lib)
// 添加第三方库文件
target_link_libraries(testProject libdarknet.so)
````
