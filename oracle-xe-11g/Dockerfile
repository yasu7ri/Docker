FROM centos:centos7

MAINTAINER Yasunari Tanaka<yasu7ri.tanaka@gmail.com>

# Pre-requirements
RUN mkdir -p /run/lock/subsys

RUN yum update -y
RUN yum install -y unzip libaio bc initscripts net-tools

# Install Oracle XE
ADD rpm/oracle-xe-11.2.0-1.0.x86_64.rpm.zip /tmp/
RUN unzip -d /tmp/ /tmp/oracle-xe-11.2.0-1.0.x86_64.rpm.zip
RUN rpm -ivh /tmp/Disk1/oracle-xe-11.2.0-1.0.x86_64.rpm

# Configure instance
ADD config/xe.rsp config/init.ora config/initXETemp.ora /u01/app/oracle/product/11.2.0/xe/config/scripts/
RUN chown oracle:dba /u01/app/oracle/product/11.2.0/xe/config/scripts/*.ora \
                     /u01/app/oracle/product/11.2.0/xe/config/scripts/xe.rsp
RUN chmod 755        /u01/app/oracle/product/11.2.0/xe/config/scripts/*.ora \
                     /u01/app/oracle/product/11.2.0/xe/config/scripts/xe.rsp
ENV ORACLE_HOME /u01/app/oracle/product/11.2.0/xe
ENV ORACLE_SID  XE
ENV PATH        $ORACLE_HOME/bin:$PATH

RUN /etc/init.d/oracle-xe configure responseFile=/u01/app/oracle/product/11.2.0/xe/config/scripts/xe.rsp

# CLEANING
RUN rm /tmp/oracle-xe-11.2.0-1.0.x86_64.rpm.zip
RUN rm -rf /tmp/Disk1

# Run script
ADD config/start.sh /
CMD /bin/bash /start.sh

EXPOSE 1521
EXPOSE 8080
