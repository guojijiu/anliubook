### 希尔排序

### 希尔排序是把记录按下标的一定增量分组，对每组使用插入排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，整个序列恰被分成一组，算法便终止。和插入排序一样，从局部到全部，希尔排序是局部再局部。

```
    public function HillSort()
    {
        $arr = [3, 8, 0, 234, 345, 3, 12, 2, 5123, 1, 7, 4];

        for ($gap =count($arr)/2; $gap>0;$gap/=2) {
            for ($i = $gap; $i < count($arr); $i++) {
                $j = $i;
                while($j-$gap>=0&&$arr[$j]<$arr[$j-$gap]){
                    $temp = $arr[$j];
                    $arr[$j] = $arr[$j-$gap];
                    $arr[$j-$gap] = $temp;
                    $j-=$gap;
                }
            }
        }
        dump($arr);
    }
```
