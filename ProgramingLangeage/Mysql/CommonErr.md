
# this is incompatible with sql_mode=only_full_group_by
SET sql_mode = 'STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

# tablespace is missing for table cloud_platform.cp_user_information

1. 备份数据库
2. 执行命令

```
REPAIR TABLE cp_user_information;
ALTER TABLE cp_user_information DISCARD TABLESPACE;
ALTER TABLE cp_user_information IMPORT TABLESPACE;
```
