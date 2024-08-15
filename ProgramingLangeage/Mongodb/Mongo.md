# 创建管理员
db.createUser({ user: 'admin', pwd: 'admin123456', roles: [ { role: "root", db: "admin" } ] });

# 验证用户
db.auth("yapi", "yapi123456!@#");

# 添加用户
db.createUser()({
    user: 'yapi',
    pwd: 'yapi123456!@#',
    roles: [
        { role: "dbAdmin", db: "yapi" },
        { role: "readWrite", db: "yapi" }
    ]
});

# 备份与恢复

备份
docker exec -i mongodb mongodump --host 127.0.0.1 --port 27017 --username root --password "123456" --authenticationDatabase admin --db cloud_platform --out /data/db/bakup/

恢复
docker exec -i mongodb mongorestore --host 127.0.0.1 --port 27017 --username root --password "123456" --authenticationDatabase admin --db cloud_platform_1 /data/db/bakup/cloud_platform/
