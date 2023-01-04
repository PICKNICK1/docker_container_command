# docker_container_command
自用docker常用镜像容器run命令


* mysql:`docker run --network mysql -p 3306:3306 --name mysql3306 -v D:\tool\docker\dockerVolumes\mysql\conf:/etc/mysql/conf.d -v D:\tool\docker\dockerVolumes\mysql\logs:/var/log/mysql -v D:\tool\docker\dockerVolumes\mysql\data:/var/lib/mysql --privileged=true -e MYSQL_ROOT_PASSWORD=xA123456 -d mysql`
* redis:`docker run -p 6379:6379 -d -v D:\tool\docker\dockerVolumes\redis:/usr/local/etc/redis --name myredis redis redis-server /usr/local/etc/redis/redis.conf`
* 查看容器启动命令  `docker run --rm -v /var/run/docker.sock:/var/run/docker.sock cucker/get_command_4_run_container 容器name`
* nacos带mysql: `docker run --network mysql --name spring_cloud_standalone_nacos -p 8848:8848 -e MODE=standalone -e SPRING_DATASOURCE_PLATFORM=mysql -e MYSQL_SERVICE_HOST=mysql3306 -e MYSQL_SERVICE_PORT=3306 -e MYSQL_SERVICE_DB_NAME=nacos_config -e MYSQL_SERVICE_USER=root -e MYSQL_SERVICE_PASSWORD=xA123456 -e MYSQL_SERVICE_DB_PARAM=characterEncoding=utf8"&"zeroDateTimeBehavior=convertToNull"&"useSSL=false"&"allowMultiQueries=true"&"useJDBCCompliantTimezoneShift=true"&"useLegacyDatetimeCode=false"&"serverTimezone=Asia/Shanghai -d nacos/nacos-server:v2.1.0`
* es和kibana:  
es: `docker run -idt --name single_es -p 9200:9200 -v D:\tool\docker\dockerVolumes\single_es\es\data:/usr/share/elasticsearch/data  -v D:\tool\docker\dockerVolumes\single_es\es\logs:/usr/share/elasticsearch/logs -e ES_JAVA_OPTS="-Xms2048m -Xmx2048m"   -e ELASTIC_CLIENT_APIVERSIONING=true elasticsearch:8.3.3`  
-e ELASTIC_CLIENT_APIVERSIONING=true开启兼容客户端模式  
进入容器: `docker exec -it single_es /bin/bash `  
重置密码: `bin/elasticsearch-reset-password -u elastic`  
生成kibana使用的token: `/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana`  
kibana: `docker run --name kibana  -p 5601:5601 -idt kibana:8.3.3`  
进入localhost:5601 输入生成的token 然后需要输入验证码,生成:`docker exec -it kibana bin/kibana-verification-code` 然后确认,后面需要输入用户名,密码(用户名为elastic,密码为重置后的密码)
* jenkins: `docker run --name my-jenkins -u root -d -p 8080:8080 -p 50000:50000  -v D:\tool\docker\dockerVolumes\jenkins:/var/jenkins_home  -v /var/run/docker.sock:/var/run/docker.sock --env JAVA_OPTS=-Dhudson.model.DownloadService.noSignatureCheck=true jenkinsci/blueocean`  
插件站点选择:https://cdn.jsdelivr.net/gh/lework/jenkins-update-center/updates/tencent/update-center.json
* rabbitMq: `docker run -d -p 15672:15672 -p 5672:5672 -p 15675:15675 -p 3001:1883 -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin --name my-rabbitmq rabbitmq:3.11.4-management `
