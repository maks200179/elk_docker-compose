FROM maxfr1158/centos7:31.19

RUN localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.UTF-8
ENV container docker


RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;


#cleane all
#RUN rm -rf /var/cache/yum
#RUN yum -y update


# Install initscripts openssh-server openssh-clients	
RUN yum -y install  openssh-server 

# Set root password
RUN echo 'root:exemple' | chpasswd




RUN yum -y install epel-release
RUN yum -y install nginx
RUN mkdir /etc/ssl/private && chmod 700 /etc/ssl/private 
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/jenkins-selfsigned.key -out /etc/ssl/certs/jenkins-selfsigned.crt  -subj "/C=US/ST=State/L=City/O=Global Security/OU=IT Department/CN=domain_name.co" 
RUN echo "nginx" > /usr/share/nginx/html/index.html
RUN systemctl enable nginx
RUN yum clean all
RUN rm -rf /var/cache/yum

# Set nginx configuration file
RUN rm /etc/nginx/nginx.conf
COPY production/nginx.conf /etc/nginx/
RUN chown root:root /etc/nginx/nginx.conf
RUN chmod -R 644 /etc/nginx/nginx.conf

# Set sshd config
RUN rm /etc/ssh/sshd_config
COPY production/sshd_config /etc/ssh/
RUN chown root:root /etc/ssh/sshd_config
RUN chmod -R 600 /etc/ssh/sshd_config




ENTRYPOINT ["/usr/sbin/init"]
VOLUME [ "/sys/fs/cgroup" ]







