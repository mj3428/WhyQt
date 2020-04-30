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
3. ui_widget.h文件
```cpp
#ifndef UI_WIDGET_H
#define UI_WIDGET_H

#include <QtCore/QVariant>
#include <QtWidgets/QAction>
#include <QtWidgets/QApplication>
#include <QtWidgets/QButtonGroup>
#include <QtWidgets/QHeaderView>
#include <QtWidgets/QLabel>
#include <QtWidgets/QPushButton>
#include <QtWidgets/QWidget>

QT_BEGIN_NAMESPACE

class Ui_Widget       //         定义一个类Ui_Widget，用于封装可视化设计的界面
{
public:
    QLabel *LabDemo;  //         在界面上每个组件定义了一个指针变量，变量名称就是设置的objectName.
    QPushButton *btnClose;

    void setupUi(QWidget *Widget)     // setupUi()函数用于创建各个界面组件，并设置其位置、大小、文字内容、字体等属性，设置信号与槽的关联
    {
        if (Widget->objectName().isEmpty())
            Widget->setObjectName(QStringLiteral("Widget"));
        Widget->resize(280, 168);
        LabDemo = new QLabel(Widget);
        LabDemo->setObjectName(QStringLiteral("LabDemo"));
        LabDemo->setGeometry(QRect(50, 20, 201, 51));
        QFont font;
        font.setPointSize(20);
        font.setBold(true);
        font.setWeight(75);
        LabDemo->setFont(font);
        btnClose = new QPushButton(Widget);
        btnClose->setObjectName(QStringLiteral("btnClose"));
        btnClose->setGeometry(QRect(150, 120, 75, 23));

        retranslateUi(Widget);
        QObject::connect(btnClose, SIGNAL(clicked()), Widget, SLOT(close()));  // 将在UI设计器里的设置的信号与槽的关联转换为语句

        QMetaObject::connectSlotsByName(Widget);  // 设置槽函数的关联方式，用于将UI设计器自动生成的组件信号的槽函数与组件信号相关联
    } // setupUi

    void retranslateUi(QWidget *Widget)     //  调用retranslateUi(Widget)，用来设置界面各组件的文字内容属性，比如标签的文字,按键的文字等
    {
        Widget->setWindowTitle(QApplication::translate("Widget", "My First Demo", Q_NULLPTR));
        LabDemo->setText(QApplication::translate("Widget", "Hello, World", Q_NULLPTR));
        btnClose->setText(QApplication::translate("Widget", "Close", Q_NULLPTR));
    } // retranslateUi

};

namespace Ui {        // 定义命名空间，并定义一个从Ui_Widget继承的类Widget
    class Widget: public Ui_Widget {};
} // namespace Ui

QT_END_NAMESPACE
#endif // UI_WIDGET_H
```
### 信号与槽
槽(slot)就是对信号响应的函数。  
信号与槽关联是用QObject::connect()函数实现的，其基本格式是:`QObject::connect(sender, SIGNAL(signal()), receiver, SLOT(slot()));`QObject是
一个静态函数，是所有Qt类的基类。  
其中,sender是发射信号的对象的名称。signal()是信号名称。receiver是接收信号的对象名称，slot()是槽函数的名称。  
SIGNAL和SLOT是Qt的宏，用于指明信号和槽，并将它们的参数转换为相应的字符串。  
比如:`QObject::connect(btnClose, SIGNAL(clicked()), Widget, SLOT(close()));`其作用就是将btnClose按钮的clicked()信号与窗体（Widget）的
槽函数close()相关联。  
- 一个信号可以连接多个槽:当一个信号与多个槽函数关联时，参函数按照建立连接时的顺序依次执行。    
- 多个信号可以连接同一个槽
- 一个信号可以连接另外一个信号，特殊需求:`connect(spinNum, SIGNAL(valueChanged(int)), this, SIGNAL(refreshInfo(int));`  
- 信号与槽的参数个数和类型需要一致，至少信号的参数不能少于槽的参数
- 在使用信号与槽的类中，必须在类的定义中加入宏Q_Object  
- 槽函数通常被立即执行，就像正常调用一个函数一样  
### 可视化生成槽函数原型和框架
查看编译生成的ui_qwdialog.h文件。构造函数里调用的setupUi()是在ui_qwdialog.h文件里实现的。查看setupUi(),其实秘密
就在于connectSlotsByName(QWDialog)函数将搜索QWDialog界面上的所有组件，将信号与槽函数匹配的信号关联起来。它假设槽函数的名称是:`void on_
<object name>_<signal name>(<signal parameters>);`  
