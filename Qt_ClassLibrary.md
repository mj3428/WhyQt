# Qt类库
QMap类即，Key和value，存储时是按照键的顺序，如果不在乎存储顺序，使用QHash会更快。  
QHash基于散列表来实现字典功能的模板类。  
QHash与QMap的功能和用法相似，区别在于:
- QHash比QMap的查找速度快；
- 在QMap上遍历时，数据项是按照键排序的，而QHash的数据项是任意顺序的；
- QMap的键必须提供"<"运算符，QHash的键必须提供"=="运算符和一个名称为qHash()的全局散列函数。  
