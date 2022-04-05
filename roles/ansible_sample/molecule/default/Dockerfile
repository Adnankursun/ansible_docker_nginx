FROM centos:8

ENV container docker


RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
  rm -f /lib/systemd/system/multi-user.target.wants/*;\
  rm -f /etc/systemd/system/*.wants/*;\
  rm -f /lib/systemd/system/local-fs.target.wants/*; \
  rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
  rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
  rm -f /lib/systemd/system/basic.target.wants/*;\
  rm -f /lib/systemd/system/anaconda.target.wants/*;
#   sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*;\
#   sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*;\
#   dnf distro-sync -y;\
#   yum -y update;\
#   yum install yum-utils -y;\
#   yum-config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo;\
#   yum install systemd -y;\
#   yum clean all;\
#   yum install systemd-libs -y;\
#   yum install docker-ce -y;\
#   yum install python2 -y;

VOLUME ["/sys/fs/cgroup"]

CMD ["/usr/sbin/init"]
# End Centos instructions for systemd container

# https://molecule.readthedocs.io/en/latest/examples.html#docker-with-non-privileged-user
# Create `ansible` user with sudo permissions and membership in `DEPLOY_GROUP`
ENV ANSIBLE_USER=ansible SUDO_GROUP=wheel DEPLOY_GROUP=deployer

RUN \
  yum -y install \
  sudo  \
  && yum clean all  \
  && rm -rf /var/cache/yum/*

RUN set -xe \
  && groupadd -r ${ANSIBLE_USER} \
  && groupadd -r ${DEPLOY_GROUP} \
  && useradd -m -g ${ANSIBLE_USER} ${ANSIBLE_USER} \
  && usermod -aG ${SUDO_GROUP} ${ANSIBLE_USER} \
  && usermod -aG ${DEPLOY_GROUP} ${ANSIBLE_USER} \
  && sed -i "/^%${SUDO_GROUP}/s/ALL\$/NOPASSWD:ALL/g" /etc/sudoers
