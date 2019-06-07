FROM centos
RUN yum install openssh-server -y
RUN ssh-keygen -A
ADD sshd_config /etc/ssh/sshd_config
RUN echo "root:ubuntu" | chpasswd
EXPOSE 22
CMD /usr/sbin/sshd -D

