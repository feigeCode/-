# 防火墙相关

**查看已经开放的端口**

firewall-cmd --list-ports

**2.开启指定端口**

firewall-cmd --zone=public --add-port=2181/tcp --permanent

**3.关闭指定端口**

firewall-cmd --zone=public --remove-port=2181/tcp --permanent


**4.重启防火墙**

firewall-cmd --reload

# Shell脚本的基本语法

~~~shell
RES1=$(((1+2)*4))
echo $RES1

RES2=$[(1+2)*4]
echo $RES2


# SUM=$[$1+$2]
# echo SUM=$SUM


if [ "ok" = "ok" ]
then
        echo equal
fi

# -f 存在且是一个常规文件
# -e 存在
# -d 存在且是一个目录
if [ -d /home/feige ]
then
        echo "/home/feige 该目录存在"
fi

# -lt -le -gt -ge -eq -ne
if [ $1 -ge 60 ]
then
        echo 及格
elif [ $1 -lt 60 ]
then
        echo 不及格

fi

# $*把输入的参数当做一个整体
# $@把输入的参数分别对待
for i in $@
do
        echo $i
done
ADD=0
for(( i=1;i<=100;i++ ))
do
        ADD=$[$ADD+$i]
done
echo ADD=$ADD


read -p "请输入一个数NUM1" NUM1

echo $NUM1
~~~



~~~shell
function add(){
        echo $[$n1+$n2]
}

read -p "请输入n1"  n1
read -p "请输入n2"  n2
add $n1 $n2
~~~



~~~shell
#!/bin/bash
# 备份目录
BACKUP=/data/backup/db
# 当前时间
DATETIME=$(date +%Y-%m-%d_%H%M%S)

echo $DATETIME

# 数据库地址
HOST=localhost
# 数据库用户名和密码
DB_USER=root
DB_PW=hufei
# 备份数据库名
DATABASE=demo

# 创建备份目录，如果不存在，就创建

[ ! -d "${BACKUP}/${DATETIME}" ] && mkdir -p "${BACKUP}/${DATETIME}"


# 备份数据库
mysqldump -u${DB_USER} -p${DB_PW} --host=${HOST} -q -R --database ${DATABASE} | gzip > ${BACKUP}/${DATETIME}.sql.gz
~~~

