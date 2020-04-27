# GUI
## UI文件与运行机制
### 项目管理文件
后缀为".pro"是项目的管理文件,内容为  
```cpp
Qt += core gui // gui模块是GUI实际的类库模块，要是控制台就不需要；项目涉及到sql就改成 += sql
greaterThan(Qt_MAJOR_VERSION, 4):Qt += widgets // 版本大于4就加入widgets模块
TARGET = samp2_1  // 目标可执行文件的名称，编译后生成为samp2_1.exe
TEMPLATE = app  //  模板为一般的应用程序application
SOURCES += main.cpp //  源文件、头文件、和窗体文件
           widget.cpp
HEADERS += widget.h
FORMS += widget.ui
```
