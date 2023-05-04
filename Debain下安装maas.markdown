Debain下 非容器安装maas
---------------------

## 更新apt
`sudo apt update`

## 安装maas
`snap install maas --devmode --stable`


## 初始化maas

选择以下模式之一决定了哪些服务将在本地系统上运行:

| Mode           | Region | Rack | Database | Description                  |
| -------------- |--------| ---- | -------- | ---------------------------- |
| `all`          |    X   |   X  |     X    | 所有服务                 |
| `region`       |    X   |      |          | 仅限区域 API 服务器       |
| `rack`         |        |   X  |          | 仅机架控制器         |
| `region+rack`  |    X   |   X  |          | 区域 API 服务器和机架控制器 |
| `none`         |        |      |          | 取消初始化 MAAS 并停止服务 |

## 安装 postgresql
`sudo apt install -y postgresql`

# 创建数据库用户名密码  
使用postgres用户  
`su postgres`

创建用户
`psql -c "CREATE USER \"maascli\" WITH ENCRYPTED PASSWORD 'maascli'"`

创建数据库
`createdb -O "maascli" "maasclidb"`

修改配置
`vi /etc/postgresql/13/main/pg_hba.conf`
添加
`host    maasclidb       maascli         0/0                     md5`


# 运行maas
`/snap/bin/maas init --mode all  --database-uri "postgres://maascli:maascli@localhost/maasclidb"`
运行后会得到
> MAAS URL [default=http://10.10.23.2:5240/MAAS]:
默认显示的是本地访问地址, 需要指定外网访问的url


# 创建MAAS后台登陆管理员账户
`/snap/bin/maas createadmin`

