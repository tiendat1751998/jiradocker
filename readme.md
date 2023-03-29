confluence
README 

default port: 8090

Long Term Support Version: v7(7.19.7)
Latest Version: v8(8.1.3)
Requirement
docker-compose: 17.09.0+
How to run with docker-compose
start confluence & mysql
git clone https://github.com/onegin/confluence.git \
&& cd confluence \
&& docker-compose up
start confluence & mysql daemon
docker-compose up -d
default db(mysql8.0) configure:
driver=mysql
host=mysql-confluence
port=3306
db=confluence
user=root
passwd=123456
How to run with docker
start confluence
docker volume create confluence_home_data && docker network create confluence-network && docker run -p 8090:8090 -v confluence_home_data:/var/confluence --network confluence-network --name confluence-srv -e TZ='Asia/Shanghai' haxqer/confluence:7.19.7
config your own db:
How to hack confluence
docker exec confluence-srv java -jar /var/agent/atlassian-agent.jar \
-p conf \
-m Hello@world.com \
-n Hello@world.com \
-o your-org \
-s you-server-id-xxxx
How to hack confluence plugin
.eg I want to use BigGantt plugin
Install BigGantt from confluence marketplace.
Find App Key of BigGantt is : eu.softwareplant.biggantt
Execute :
docker exec confluence-srv java -jar /var/agent/atlassian-agent.jar \
-p eu.softwareplant.biggantt \
-m Hello@world.com \
-n Hello@world.com \
-o your-org \
-s you-server-id-xxxx
Paste your license
How to upgrade
cd confluence && git pull
docker pull onegin/confluence:latest && docker-compose stop
docker-compose rm
enter y, then start server

docker-compose up -d