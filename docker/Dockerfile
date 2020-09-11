FROM centos:centos7
ENV HOME /root

RUN yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN rpm --import http://linuxsoft.cern.ch/wlcg/RPM-GPG-KEY-wlcg
RUN yum -y install http://linuxsoft.cern.ch/wlcg/centos7/x86_64/wlcg-repo-1.0.0-1.el7.noarch.rpm
RUN yum -y install alicexrdplugins

RUN yum -y install sudo vim htop git curl.x86_64 bind-utils.x86_64 \
        redhat-lsb.x86_64 bzip2.x86_64 libicu libpcap initscripts \
        libstdc++-devel net-tools cmake3.x86_64 xrootd-client.x86_64 \
        xrootd-client-devel.x86_64 xrootd-client-libs.x86_64 \
        xrootd-devel.x86_64 xrootd-libs.x86_64 xrootd-server.x86_64 \
        xrootd-server-devel.x86_64 xrootd-server-libs.x86_64 \
        centos-release-scl tree rpmdevtools rpmdev-setuptree chrpath \
        dos2unix bzip2-devel expat-devel libicu-devel python-devel \
        docbook-style-xsl doxygen libxslt texlive-latex sqlite-devel \
        openssl-devel libpcap-devel

RUN yum -y install devtoolset-8-gcc*

RUN sudo sh -c "printf '\
export PATH=\$PATH:/opt/rh/devtoolset-8/root/usr/bin\n\
scl_source enable devtoolset-8\
' > /etc/profile.d/container.sh"

# Install NDN Software Stack
WORKDIR ${HOME}
RUN git clone https://github.com/cmscaltech/sandie-ndn.git
RUN git clone https://github.com/cmscaltech/sandie-ndn-repo.git
