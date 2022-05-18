---
title: RationalRose的小坑
tags: UML
article_header:
  type: cover
  image:
    src: /img/uml/logo.png

---

Rational Rose正向生成类时的小坑



<!--more-->

## 问题记录

![](https://pic.imgdb.cn/item/62845ded0947543129975908.jpg)

## 问题解决

设置默认语言为Java，Tools->Options->Notation->default：选择Java。



## 正确的使用方法 

一 正向工程

  1、设置默认语言为Java，Tools->Options->Notation->default：选择Java。

  2、设置[环境变量](https://so.csdn.net/so/search?q=环境变量&spm=1001.2101.3001.7020)ClassPath，Tools->Java/j2ee->Project  Specification->ClassPath：具体路径设置为正向工程生成java文件要保存的目录，一般为项目的src目录。

  3、打开设计好的[类图](https://so.csdn.net/so/search?q=类图&spm=1001.2101.3001.7020)，选中要生成的Java文件的类，然后通过Tools->Java/J2ee->General  Code生成java文件.

  4、正向工程注意事项：

  以上是正向工程的操作流程，过程比较简单，主要是操作过程中以及设计类时有些问题大家需要注意一下，以后实际操作时会节省一些时间，主要有以下几点：

  1）.生成代码前将Project  Specifiction属性页Code  Generation标签项中的Generate  Rose  ID  和  Generate  Default  Return  Line两个复选框的默认选中状态去掉，以免生成一些我们不需要的信息

  2）.设计model等值对象时,不必为其设计getter(),setter()方法，将对应字段属性设置为：proerty  type:simple  即可，正向工程会自动生成其getter,setter方法。

  3）.类之间调用关系的设计：

  一般A类调用B类，最终代码中经常以在A类里初始化一个b类的变量。在设计时，不要在A类中设计一个B类类型的属性。这种关系要在Association  Specification中通过为Role  A指定值来实现。

  4）.类设计时要按开发规范写好类和方法的注释，正向工程会将注释生成到代码中，开发过程中注释如有改动，可通过逆向工程将类图和代码保持同步。

  二 逆向工程操作流程

  1.点击Tools->Java/[J2ee](https://so.csdn.net/so/search?q=J2ee&spm=1001.2101.3001.7020)->Reverse  Engineer，调出Java  Reverse  Engineer对话框。

  2、在此页面添加要进行逆向工程的Java文件，并选中，然后点击Reverse按钮即可。



## 参考资料

[使用Rational Rose由代码生成类图](https://blog.csdn.net/liaibao121/article/details/6827546)