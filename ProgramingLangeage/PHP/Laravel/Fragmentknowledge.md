### 碎片知识

1. 依赖注入

```
实质上类的依赖通过构造函数，或者某些情况下通过setter方法注入类中；

```

2. unicode内容，解决编码转中文：

```
$str = '';(unicode码内容)
preg_replace_callback('/\\\\u([0-9a-f]{4})/i', function ($match) {
            return mb_convert_encoding(pack('H*', $match[1]), 'UTF-8', 'UCS-2BE');
        }, $str);
```