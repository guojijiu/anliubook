### 快速排序

### 将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。体现出分治的思想。

```
    public function QuickSort(array $arr)
    {
        $count = count($arr);
        if ($count < 2) {
            return $arr;
        }
        //创建临时数组，以基准值为分界线，大于基准值的放在右侧，小鱼基准值的放在左侧
        $leftArr = $rightArr = [];
        //基准值，一般取数组第一个元素
        $middle = $arr[0];
        //循环数组与基准值比较
        for ($i = 1; $i < $count; $i++) {
            if ($arr[$i] < $middle) {
                $leftArr[] = $arr[$i];
            } else {
                $rightArr[] = $arr[$i];
            }
        }
        //递归，将左右数组排序
        $leftArr = $this->QuickSort($leftArr);
        $rightArr = $this->QuickSort($rightArr);

        //将排好序的临时数组合并
        return array_merge($leftArr, [$middle], $rightArr);
    }
```
