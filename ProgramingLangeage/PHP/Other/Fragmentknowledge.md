### 碎片知识

1. php反射类和实例化类的区别

```
PHP的反射类与实例化对象作用相反，实例化是调用封装类中的方法、成员，而反射类则是拆封类中的所有方法、成员变量，并包括私有方法等。就如“解刨”一样，我们可以调用任何关键字修饰的方法、成员。
```

2. tcpdf扩展安装字体

```
将字体放大哦/vendor/tecnickcom/tcpdf/tools，并执行
./tcpdf_addfont.php -b -t TrueTypeUnicode -i wryh.ttf
```
