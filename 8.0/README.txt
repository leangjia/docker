
基于wdmsyf的修改，增加odoo8.0的国内源，加快编译速度：


---------------------------------
1、
获取Dockerfile及相关文件

git clone https://github.com/wdmsyf/docker.git
进入odoo10目录：
cd 8.0



2、编译镜像

docker build --force-rm -t wdmsyf/odoo:8.0 .



3、创建容器

由于本镜像不含PostgreSQl，所以需要先创建一个PostgreSQL容器：
docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo --name odoo_db postgres:9.4


然后创建odoo容器，数据库则连接到刚才创建的PostgreSQL容器，8069端口映射到宿主机的8869，8071映射到宿主机的8871：
a、
使用镜像内官方的源代码和默认的配置文件：
docker run -d --name odoo8_wdmsyf -p 8869:8069 -p 8871:8071 --link odoo_db:db wdmsyf/odoo:8.0

b、
使用自定义的配置文件，配置文件为 ~/odoo_conf/openerp-server.conf，配置文件在宿主机路径可变，文件名不能变：
docker run -d --name odoo8_wdmsyf -p 8869:8069 -p 8871:8071 --link odoo_db:db -v ~/odoo_conf:/etc/odoo wdmsyf/odoo:8.0

c、
使用自定义配置文件和自定义odoo源代码（比如可以换成企业版），配置文件在宿主的路径可变，文件名不能变；源代码在宿主机的路径可变：
docker run -d --name odoo8_wdmsyf -p 8869:8069 -p 8871:8071 --link odoo_db:db -v ~/odoo_conf:/etc/odoo -v ~/odoo_src/odoo:/usr/lib/python2.7/dist-packages/odoo wdmsyf/odoo:8.0










=============================
本镜像由康虎软件工作室创建并提供

康虎软件工作室提供的服务：
* odoo实施、顾问服务
* 专业web打印解决方案。我工作室提供的康虎云报表系统可以让浏览器打印达到C/S系统打印的效果和体验。

* 康虎软件工作室：
* website: http://www.cfsoft.cf
* QQ：     360026606
* 微信：   360026606


