### 计数排序

### 假如输入一个数x，如果我们可以找到比该数小的数有几个，那么就可以直接将x放入到对应的输出数组的位置。 比如测试序列中的x=5，，比5小的有2个，那么毫无疑问5就该排在第三位。

```
（未验证）
function countingSort($arr, $maxValue = null)
{
    if ($maxValue === null) {
        $maxValue = max($arr);
    }
    for ($m = 0; $m < $maxValue + 1; $m++) {
        $bucket[] = null;
    }

    $arrLen = count($arr);
    for ($i = 0; $i < $arrLen; $i++) {
        if (!array_key_exists($arr[$i], $bucket)) {
            $bucket[$arr[$i]] = 0;
        }
        $bucket[$arr[$i]]++;
    }

    $sortedIndex = 0;
    foreach ($bucket as $key => $len) {
        if ($len !== null) $arr[$sortedIndex++] = $key;
    }

    return $arr;
}
```
