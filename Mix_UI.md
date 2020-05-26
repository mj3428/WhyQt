# 手工创建的组件的信号与槽
```cpp


void QWMainWind::iniSignalSlots()
{ //信号与槽的关联，当函数带有参数时，必须写明参数的类型
    connect(spinFontSize,SIGNAL(valueChanged(int)),
            this,SLOT(on_spinBoxFontSize_valueChanged(int)));

    connect(comboFont,SIGNAL(currentIndexChanged(const QString &)),
            this,SLOT(on_comboFont_currentIndexChanged(const QString &)));
}

void QWMainWind::on_spinBoxFontSize_valueChanged(int aFontSize)
{//改变字体大小的SpinBox的响应
    QTextCharFormat fmt;
    fmt.setFontPointSize(aFontSize); //设置字体大小
    ui->txtEdit->mergeCurrentCharFormat(fmt);
    progressBar1->setValue(aFontSize);
}

void QWMainWind::on_comboFont_currentIndexChanged(const QString &arg1)
{//FontCombobox的响应，选择字体名称
    QTextCharFormat fmt;
    fmt.setFontFamily(arg1);//设置字体名称
    ui->txtEdit->mergeCurrentCharFormat(fmt);
}

```
