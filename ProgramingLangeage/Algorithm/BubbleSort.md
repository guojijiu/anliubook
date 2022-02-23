### 冒泡排序

### 通过对乱序序列从前向后遍历,依次比较相邻元素的值，若发现逆序则交换，使值较大的元素逐渐从前移向后部。像水底下的气泡一样逐渐向上冒一样。

```
    public function bubbleSort()
    {
        $arr = [3, 8, 0, 234, 345, 3, 12, 2, 5123, 1, 7, 4];

        for ($i = 0; $i < count($arr); $i++) {
            for ($j = 0; $j < count($arr) - 1; $j++) {
                if ($arr[$j] > $arr[$j + 1]) {
                    $tmp = $arr[$j];
                    $arr[$j] = $arr[$j + 1];
                    $arr[$j + 1] = $tmp;
                }
            }
        }
        dump($arr);
    }
```
