### 分析sql日志

### laravel利用 Spreadsheet包将分析的结果输出到excel表格中

```php
        $file = fopen("/Users/liuwei/Downloads/1102.txt", "r");
        $fileArr = [];
        $i = 0;
        while (!feof($file)) {
            $fileArr[$i] = fgets($file);
            $i++;
        }
        fclose($file);
        $fileArr = array_filter($fileArr);
        $result = [];
        foreach ($fileArr as $key => $item) {
            $sql = rtrim(substr($item, strripos($item, "ms:") + 4), "\n");
            $info = substr($item, 0, strrpos($item, "s:") + 1);
            $info = explode(" ", $info);
            foreach ($info as &$item) {
                if (strpos($item,"[" ) !== false) {
                    $item = ltrim("[", $item);
                }
                if (strpos($item,"]") !== false) {
                    $item = rtrim("]", $item);
                }
            }
            $result[$key] = $info;
            array_push($result[$key], $sql);
            $explain = DB::connection("mysql")->select("explain " . $sql);
            $table = $explain[0]->table;
            $type = $explain[0]->type;
            $possible_keys = $explain[0]->possible_keys;
            $ref = $explain[0]->ref;
            $rows = $explain[0]->rows;
            array_push($result[$key], $table);
            array_push($result[$key], $type);
            array_push($result[$key], $possible_keys);
            array_push($result[$key], $ref);
            array_push($result[$key], $rows);
            array_push($result[$key], var_export($explain, true));

        }
        $spreadsheet = new Spreadsheet();//实例化
        $spreadsheet->setActiveSheetIndex(0);//设置excel的索引
        $sheet = $spreadsheet->getActiveSheet();
        /*设置单元格列宽*/
        $sheet->getColumnDimension('A')->setWidth(20);
        $sheet->getColumnDimension('B')->setWidth(15);
        $sheet->getColumnDimension('C')->setAutoSize(true);
        /*设置字体大小*/
        $sheet->getStyle('A1:c1')->getFont()->setBold(true)->setName('Arial')->setSize(10);
        //锁定表头
        $sheet->freezePane('A2');
        $sheet->setCellValue('A1', '日期')
            ->setCellValue('B1', '时间')
            ->setCellValue('C1', '级别')
            ->setCellValue('D1', '耗时')
            ->setCellValue('E1', '类型')
            ->setCellValue('F1', 'sql')
            ->setCellValue('G1', '表名')
            ->setCellValue('H1', '分析类型')
            ->setCellValue('I1', '可能用到的索引')
            ->setCellValue('J1', '索引寻找')
            ->setCellValue('K1', '检索行数')
            ->setCellValue('L1', 'explain');

        $sheet->fromArray($result, null, 'A2');
        $writer = new Xls($spreadsheet);
        $pathUrl = public_path() . '/excel/';
        $filePath = $pathUrl . 'test.xlsx';
        //判断目录是否存在，如果不存在就新建
        if (!is_dir($pathUrl))
            mkdir($pathUrl, 0755, true);
        $writer->save($filePath);
```
