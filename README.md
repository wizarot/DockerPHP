#  基于Docker容器化的PHP开发环境

---

## 快速开始

当前项目的组成：

- Ubuntu
- Nginx 2.2
- MySQL 5.6
- PHP 5.6 
- Redis 3.0
- composer
- Beanstalkd (可选)


需要的工具：

- docker
- docker-compose

1.下载项目
 
	$ git clone git@github.com:wizarot/DockerPHP.git

2.根据需要配置文件 docker-composer.yml
	
	app:
	  build: debian
	  # tty: true
	  volumes:
	    - ./php-project-path:/apps #配置项目目录
    
./data/ 目录是mysql数据库文件目录
./logs/ 存放运行日志

数据库用户名,密码 root root 这些都可以在 ./mysql目录下配置文件中找到

可以使用多个项目,通过域名解析方式,例子在./nginx/php56/ 中
可以根据需要增加php54或php70. 按照例子自己配置好了.有问题可以google查~ 基本都有解答

3.构建docker镜像,如果不是很确定,麻烦先别乱改配置

	$ docker-compose build

4.启动docker组件
	
	$ docker-compose up -d

5. 查看是否正确运行

	$ docker-compose ps 

正常应当看到几个组件都已运行

6.修改php,nginx等配置后执行
	$ docker-compose restart
修改docker配置,尽量重建docker,再尝试
	$ docker-compose build

7.偶尔需要进哪个组件执行命令行可以尝试:
	$ dockker-compose run php56 bash
这样的命令,因为 php56中安装的composer,如果需要执行composer update 之类的,需要这样进去到对应目录执行. composer 已经装好了.

## 项目结构 我的构建原则

a.log日志文件必须放在一起
b.config 配置文件需要分别放,跟着组件走
c.php-fpm nginx redis mysql 分别使用docker容器,各用各的,这样方便随时使用或者更换需要的
d.项目放在 可以自定义的 目录下,具体位置可以自行配置
e.mysql的数据文件要单独出来,防止摧毁docker后数据库也要重建,放data目录
f.composer 必须得有啊,还要想办法和命令行引出来用.
g.使用Ubuntu系统

