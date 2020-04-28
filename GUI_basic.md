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
### 窗体相关的文件
1. widget.h文件
widget.h文件是窗体类的头文件。在创建项目时，选择窗体基类是QWidget，在widget.h中定义了一个继承自QWidget的类Widget.  
```cpp
// widget.h内容 //
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>

namespace Ui {
class Widget;
}

class Widget : public QWidget
{
    Q_OBJECT

public:    // 定义了Widget类的构造函数和析构函数
    explicit Widget(QWidget *parent = 0);
    ~Widget();

private:   // 
    Ui::Widget *ui;
};

#endif // WIDGET_H
```
Widget类的定义。widget.h主题部分是一个继承于QWidget的类Widget的定义，也就是本实例的窗体类。  
2. widget.cpp文件
```cpp
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent) :    // 构造函数头部
    QWidget(parent),  // 意义是执行父类QWidget的构造函数，创建一个Ui::Widget类的对象ui。
    ui(new Ui::Widget)    // 这个ui就是widget的private部分定义的指针变量ui
{
    ui->setupUi(this);
}

Widget::~Widget()
{
    delete ui;
}
```
