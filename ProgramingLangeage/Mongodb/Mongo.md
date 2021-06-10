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
