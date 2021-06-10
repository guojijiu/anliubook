### 数据库备份和恢复

### 导出
mysqldump -u root -p  mw_tools > 2021_05_27_mw_tools_dump.sql

### 将备份数据迁移至新服务器
scp -P 9422 2021_05_27_mw_tools_dump.sql  deploy@121.36.85.1:/home/deploy

### 目标主机

### 1、登录 mysql
mysql -u root -p16399

### 2、创建 student_db 数据库
mysql > create database mw_tools_bak  default character set utf8mb4 collate utf8mb4_unicode_ci; 

### 3、切换 数据库
mysql > use mw_tools_bak

### 4、导入
mysql > source  /home/deploy/2021_05_27_1_mw_tools_dump.sql