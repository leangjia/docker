
基于官方Docker修改，修复或增强以下功能：


1、升级xlrd到1.0.0版，以解决导入excel文件时中文出错（UnicodeEncodeError: 'ascii' codec can't encode characters in position...）
升级办法：
在Docker中第22行
  && pip install psycogreen==1.0
后面增加
  && pip install xlrd=1.0.0


2、把debian的软件源改成国内源，大大加快创建镜像的速度


3、通过例子让用户可以使用保存在宿主机上的odoo源代码、配置文件


---------------------------------
1、
获取Dockerfile及相关文件

git clone https://github.com/wdmsyf/docker.git
进入odoo10目录：
cd 10.0



2、编译镜像

docker build --force-rm -t wdmsyf/odoo:10.0 .



3、创建容器

由于本镜像不含PostgreSQl，所以需要先创建一个PostgreSQL容器：
docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo --name odoo_db postgres:9.4


然后创建odoo容器，数据库则连接到刚才创建的PostgreSQL容器，8069端口映射到宿主机的8869，8071映射到宿主机的8871：
a、
使用镜像内官方的源代码和默认的配置文件：
docker run -d --name odoo10_wdmsyf -p 8869:8069 -p 8871:8071 --link odoo_db:db wdmsyf/odoo:10.0

b、
使用自定义的配置文件，配置文件为 ~/odoo_conf/odoo.conf，配置文件在宿主机路径可变，文件名不能变：
docker run -d --name odoo10_wdmsyf -p 8869:8069 -p 8871:8071 --link odoo_db:db -v ~/odoo_conf:/etc/odoo wdmsyf/odoo:10.0

c、
使用自定义配置文件和自定义odoo源代码（比如可以换成企业版），配置文件在宿主的路径可变，文件名不能变；源代码在宿主机的路径可变：
docker run -d --name odoo10_wdmsyf -p 8869:8069 -p 8871:8071 --link odoo_db:db -v ~/odoo_conf:/etc/odoo -v ~/odoo_src/odoo:/usr/lib/python2.7/dist-packages/odoo wdmsyf/odoo:10.0










=============================
本镜像由康虎软件工作室创建并提供

康虎软件工作室提供的服务：
* odoo实施、顾问服务
* 专业web打印解决方案。我工作室提供的康虎云报表系统可以让浏览器打印达到C/S系统打印的效果和体验。

* 康虎软件工作室：
* website: http://www.cfsoft.cf
* QQ：     360026606
* 微信：   360026606


