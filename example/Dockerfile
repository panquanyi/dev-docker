FROM centos:centos7.9.2009
LABEL maintainer="panquanyi"
ARG DEPLOY_ENV

ENV MAVEN_OPTS=-Xmx3000m
ENV PATH=/opt/maven/bin:$PATH
ENV TZ=Asia/Shanghai

WORKDIR /root

# NOTE detail list for display and use locally, not optimized for repo

# 安装中文包
RUN yum install -y kde-l10n-Chinese
# 重新安装glibc-common
RUN yum install -y glibc-common
# 编译生成语言库
RUN localedef -c -f UTF-8 -i zh_CN zh_CN.UTF-8
# 设置语言默认值为中文，时区改为东八区
RUN echo 'LANG="zh_CN.UTF-8"' > /etc/locale.conf
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENV LANG=zh_CN.UTF-8
ENV LC_ALL zh_CN.UTF-8

RUN yum install -y wget
#RUN mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
#RUN wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
#RUN yum clean all && yum makecache

RUN sed -i 's/^tsflags=nodocs/#tsflags=nodocs/g' /etc/yum.conf

RUN yum install -y tree psmisc which expect net-tools ntp man man-pages
RUN yum install -y gcc vim zip unzip git
RUN yum install -y openssh-server openssh-clients
RUN yum install -y java-1.8.0-openjdk

RUN wget -O apache-maven-3.6.3-bin.tar.gz --no-check-certificate \
    https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
RUN tar xzf apache-maven-3.6.3-bin.tar.gz 
RUN mkdir -p /opt && mv apache-maven-3.6.3 /opt && ln -s /opt/apache-maven-3.6.3 /opt/maven
RUN rm apache-maven-3.6.3-bin.tar.gz

RUN ssh-keygen -t rsa -N '' -f /etc/ssh/ssh_host_rsa_key
RUN ssh-keygen -t rsa -N '' -f /etc/ssh/ssh_host_ecdsa_key
RUN ssh-keygen -t rsa -N '' -f /etc/ssh/ssh_host_ed25519_key
RUN mkdir /root/.ssh && touch /root/.ssh/authorized_keys

#root用户默认密码为root
RUN echo Admin@123 | passwd --stdin root

RUN yum clean packages

EXPOSE 22

CMD /usr/sbin/init
