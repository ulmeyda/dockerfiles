FROM centos:centos6
MAINTAINER Shinya Sato version: 0.1
 
# disable selinux
RUN echo "SELINUX=disabled" > /etc/selinux/config

#-------------------------------------------
# yum (remi, epel)
#-------------------------------------------
RUN yum -y install wget
RUN wget http://ftp-srv2.kddilabs.jp/Linux/distributions/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN rpm -i epel-release-6-8.noarch.rpm
RUN rm -f epel-release-6-8.noarch.rpm

RUN wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
RUN rpm -i remi-release-6.rpm
RUN rm -f remi-release-6.rpm

RUN yum --enablerepo=remi -y update
RUN yum --enablerepo=remi,epel -y install sudo passwd zsh tmux git tig ntp
RUN yum --enablerepo=remi -y install httpd


#-------------------------------------------
# ntp
#-------------------------------------------
RUN yum --enablerepo=remi,epel -y install ntp
RUN ntpdate ntp.nict.jp
RUN sed -ri 's/server 0.centos.pool.ntp.org/server -4 ntp.nict.jp/g' /etc/ntp.conf
RUN sed -ri 's/server 1.centos.pool.ntp.org/server -4 ntp1.jst.mfeed.ad.jp/g' /etc/ntp.conf
RUN sed -ri 's/server 2.centos.pool.ntp.org/server -4 ntp2.jst.mfeed.ad.jp/g' /etc/ntp.conf
RUN sed -ri 's/server 3.centos.pool.ntp.org/server -4 ntp3.jst.mfeed.ad.jp/g' /etc/ntp.conf


#-------------------------------------------
# zsh 5.0.2
#-------------------------------------------
RUN wget ftp://ftp.pbone.net/mirror/ftp5.gwdg.de/pub/opensuse/repositories/home:/hyqhyq_3/CentOS_CentOS-6/x86_64/zsh-5.0.2-14.1.x86_64.rpm
RUN rpm -Uvh zsh-5.0.2-14.1.x86_64.rpm
RUN chsh -s /bin/zsh
RUN rm -f zsh-5.0.2-14.1.x86_64.rpm


#-------------------------------------------
# vim 7.4
#-------------------------------------------
RUN yum --enablerepo=remi -y install which gtk2 libICE libSM libXt gpm
RUN git clone https://github.com/runsisi/vim7.4-rpms-for-centos6.x.git
RUN rpm -Uvh vim7.4-rpms-for-centos6.x/vim-7.4.160-1.el6.x86_64/*
RUN rm -rf vim7.4-rpms-for-centos6.x


#-------------------------------------------
# PHP
#-------------------------------------------
RUN yum --enablerepo=remi -y install php php-pear php-gd php-mysql php-intl php-xml php-xmlrpc php-soap php-pecl-apc php-odbc php-mbstring
RUN sed -ri 's/;date.timezone =/date.timezone = Asia\/Tokyo/g' /etc/php.ini
RUN sed -ri 's/;mbstring.language =/mbstring.language =/g' /etc/php.ini
RUN sed -ri 's/;mbstring.internal_encoding = EUC\-JP/;mbstring.internal_encoding = UTF\-8/g' /etc/php.ini
RUN sed -ri 's/;mbstring.http_input =/mbstring.http_input =/g' /etc/php.ini
RUN sed -ri 's/;mbstring.http_output = SJIS/mbstring.http_output = auto/g' /etc/php.ini

#-------------------------------------------
# Composer
#-------------------------------------------
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer
 
 
#-------------------------------------------
# SSHD
#-------------------------------------------
RUN yum --enablerepo=remi install -y openssh-clients
RUN yum --enablerepo=remi install -y openssh-server
RUN echo 'root:root' |chpasswd

# Set up SSHD config
RUN sed -ri 's/#PermitRootLogin yes/PermitRootLogin yes/g' /etc/ssh/sshd_config
 
# Init SSHD
RUN /etc/init.d/sshd start
RUN /etc/init.d/sshd stop


#-------------------------------------------
# Mysql
#-------------------------------------------
RUN yum --enablerepo=remi -y install mysql mysql-server
#RUN service mysqld start && /usr/bin/mysqladmin -u root password "root"


#-------------------------------------------
# chkconfig
#-------------------------------------------
RUN chkconfig httpd on
RUN chkconfig sshd on
RUN chkconfig mysqld on
RUN chkconfig ntpd on


# yum clean
RUN yum clean all
 
EXPOSE 22
CMD /usr/sbin/sshd -D
