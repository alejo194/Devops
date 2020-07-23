## 安装参考
https://blog.csdn.net/liumiaocn/article/details/86173195 

docker jenkins 参数说明
JENKINS_MODE	JENKINS模式，可设定为master或者slave
JENKINS_ADMIN_ID	登陆用户ID
JENKINS_ADMIN_PW	登陆用户密码
JENKINS_MASTER_URL	slave方式启动时jnlpurl设定
JENKINS_SLAVE_SECRET	slave方式启动时secret设定
JENKINS_SLAVE_WORKDIR	slave方式启动时工作目录设定

master上，docker-compose.yml
version: '2'
services:
  # jenkins service based on Jenkins LTS version
  jenkins:
    image: liumiaocn/jenkins:2.150.1
    ports:
      - "32002:8080"
      - "50000:50000"
    environment:
      - JENKINS_ADMIN_ID=root
      - JENKINS_ADMIN_PW=liumiaocn
      - JENKINS_MODE=master
    volumes:
      - ./data/:/data/jenkins
    restart: "no"

创建slave节点，现在master上jenkins网页上，jenkins-->系统管理-->节点管理-->创建节点
version: '2'
services:
  # jenkins service based on Jenkins LTS version
  jenkins:
    image: liumiaocn/jenkins:2.150.1
    environment:
      - JENKINS_MODE=slave
      - JENKINS_MASTER_URL=http://192.168.163.118:32002/computer/agent001/slave-agent.jnlp
      - JENKINS_SLAVE_SECRET=48de37b3bb2cbf61b2ff7eca62b692a436f54d6bf930e4e6a6f8b1d5f64fa4be
      - JENKINS_SLAVE_WORKDIR=/tmp/agent/jenkins
    volumes:
      - ./data/:/data/jenkins
    restart: "no"
