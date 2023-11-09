# Odoo V17.0 Installation on CentOS Stream 9

## 1. 下载Odoo源码
```bash
git clone -b 17.0 https://github.com/odoo/odoo.git
# 或者
wget https://nightly.odoo.com/17.0/nightly/src/odoo_17.0.latest.tar.gz
```
github源码包很大，下载后可以删除.git目录，不要历史文件，以减少大小。
## 2. OS环境准备
```bash
dnf update -y
reboot
```
- 新增用户odoo， 作为odoo软件运行用户
```bash
[root@localhost ~]# useradd odoo
```
- 新建以下目录
```bash
[odoo@localhost ~]$ ll
总用量 4
drwxr-xr-x. 2 odoo odoo    6 11月  8 21:44 addons
drwxr-xr-x. 2 odoo odoo    6 11月  8 18:16 bin
drwxr-xr-x. 2 odoo odoo    6 11月  8 18:17 data
drwxr-xr-x. 2 odoo odoo    6 11月  8 18:17 etc
drwxr-xr-x. 2 odoo odoo    6 11月  8 18:16 log
drwxr-xr-x. 9 odoo odoo 4096 11月  8 20:53 odoo
drwxr-xr-x. 2 odoo odoo   72 11月  8 21:43 software
```

## 3. Python 准备
Odoo v17.0 requires Python 3.10 or later to run.
```bash
python3 --version
pip3 --version
```
```bash
yum install python3.11
yum install python3.11-pip
```
更改pypi源到国内
```bash
pip3.11 config set global.index-url https://mirrors.aliyun.com/pypi/simple
pip3.11 config set install.trusted-host mirrors.aliyun.com
```

## 4. PostgreSQL 准备
```bash
yum install postgresql-server postgresql
```
初始化数据库:
```bash
/usr/bin/postgresql-setup --initdb
```
```bash
systemctl start postgresql.service
systemctl enable postgresql.service
```
修改密码：
```sql
ALTER USER postgres WITH PASSWORD 'NewPassword';
```
切换到postgres用户（便于认证）下执行：
```bash
createuser -s odoo
createdb --owner odoo odoo
```

## 5. 依赖包安装
```bash
yum install openldap-devel libpq-devel cyrus-sasl-devel
yum install gcc
yum install python3.11-devel
```
```bash
# 先激活python3.11虚拟环境
source venv/bin/activate
cd /CommunityPath
pip install -r requirements.txt
 ```
 安装 wkhtmltopdf
 ```bash
 wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-3/wkhtmltox-0.12.6.1-3.almalinux9.x86_64.rpm
 dnf localinstall -y wkhtmltox-0.12.6.1-3.almalinux9.x86_64.rpm
 wkhtmltopdf --version
 ```

## 6. 运行
初始化odoo数据库（不带demo测试数据）：
```bash
cd /CommunityPath
python3 odoo-bin --addons-path=addons -d odoo -i base --without-demo=all --data-dir /home/odoo/data --stop-after-init
```
运行命令：
```bash
cd /CommunityPath
python3 odoo-bin --addons-path=addons -d mydb
# 或者
python3 odoo-bin -c etc/odoorc.cfg
```

默认密码： admin/admin
