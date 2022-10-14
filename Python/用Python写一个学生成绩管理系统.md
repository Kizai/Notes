## Python学生成绩管理系统：定义学生类，包括姓名学号年龄成绩，再定义班级类，班级名，全体学生，可以实现添加、查找、删除、按条件排序等功能。
代码如下：

```python
# _*_ coding utf-8 _*_
# 作者：KiZai
# 开发时间: 2020/5/23 21:15
# 文件名: 学生成绩管理系统 .py
# 开发工具: PyCharm
# 学生类
class Student:
    def __init__(self, name, num, age, score):
        self.name = name
        self.num = num
        self.age = age
        self.score = score

    def __str__(self):
        return '姓名:{} 学号:{} 年龄:{} 成绩:{}'.format(self.name, self.num, self.age, self.score)


# 班级类
class Class:
    def __init__(self, name):
        self.name = name
        self.stu_list = []
        self.stu_dict = {}

    # 添加学生
    def add_stu(self, stu):
        self.stu_list.append(stu)
        self.stu_dict[stu.num] = stu

    # 删除学生
    def del_stu(self, num):
        # 从字典中弹出并删除
        s = self.stu_dict.pop(num)
        # 从列表中删除
        self.stu_list.remove(s)

    # 学生排序
    def sort_stu(self, key=None, reverse=False):
        self.stu_list.sort(key=key, reverse=reverse)

    # 查找学生
    def get_stu(self, num):
        return self.stu_dict.get(num)

    # 展示学生信息
    def show_stu(self):
        for s in self.stu_list:
            print(s)


# 创建班级对象
MyClass = Class('17电四')


def ShowUI():
    print("*" * 50)
    print("1.添加学生")
    print("2.删除学生")
    print("3.查看学生")
    print("4.查找学生")
    print("5.按照成绩排序")


while True:
    try:
        ShowUI()
        key = int(input("请输入功能:"))
        print("*" * 50)
        if key in range(0, 6):
            if key == 1:
                print("1.添加学生信息:")
                name = input("姓名:")
                num = int(input("学号:"))
                age = int(input("年龄:"))
                score = int(input("成绩:"))
                stu = Student(name, num, age, score)
                MyClass.add_stu(stu)
            elif key == 2:
                print("2.删除学生信息:")
                num = int(input("请输入学号(删除):"))
                MyClass.del_stu(num)
            elif key == 3:
                print("3.学生列表:")
                MyClass.show_stu()
            elif key == 4:
                print("4.查找学生")
                num = int(input("请输入学号(查找):"))
                s = MyClass.get_stu(num)
                print(s)
            else:
                print("5.按条件排序，请选择数字来选择条件（1：按照分数排列，2：按照年龄排列，3：按照学号排列）")
                px = int(input("请输入排序条件："))
                try:
                    if px == 1:
                        MyClass.sort_stu(key=lambda s: s.score, reverse=True)  # 按照分数排列
                        MyClass.show_stu()
                    elif px == 2:
                        MyClass.sort_stu(key=lambda s: s.age, reverse=True)  # 按照年龄排列
                        MyClass.show_stu()
                    elif px == 3:
                        MyClass.sort_stu(key=lambda s: s.num, reverse=True)  # 按照学号排列
                        MyClass.show_stu()
                except:
                    print("不存在此条件，您的输入有误")
        else:
            print("输入错误!")
    except:
        print("非法输入")

```
运行截图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525224231977.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020052522445032.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525224513962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525224540479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020052522460220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)