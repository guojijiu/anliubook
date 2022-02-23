### 选择排序

### 选择排序和冒泡排序很像，是从乱序序列的数据中，按指定的规则选出某一元素，再依规定交换位置后达到排序的目的。

```
    public function SelectSort()
    {
        $arr = [3, 8, 0, 234, 345, 3, 12, 2, 5123, 1, 7, 4];

        for ($i = 0; $i < count($arr); $i++) {
            for ($j = $i; $j < count($arr); $j++) {
                if ($arr[$i] > $arr[$j]) {
                    $tmp = $arr[$i];
                    $arr[$i] = $arr[$j];
                    $arr[$j] = $tmp;
                }
            }
        }
        dump($arr);
    }
```
