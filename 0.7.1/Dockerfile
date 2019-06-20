#
#  Licensed to the Apache Software Foundation (ASF) under one or more
#  contributor license agreements.  See the NOTICE file distributed with
#  this work for additional information regarding copyright ownership.
#  The ASF licenses this file to You under the Apache License, Version 2.0
#  (the "License"); you may not use this file except in compliance with
#  the License.  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
#  limitations under the License.
#

FROM centos:7
LABEL maintainer="m9rco <pushaowei0727@gmail.com>"

ADD rootfs /
WORKDIR /root

# Create a metron user
RUN adduser metron; \
    echo metron | passwd metron --stdin; \
    usermod -aG wheel metron;

# Update 163 Images
RUN mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup \
    && curl -o /etc/yum.repos.d/CentOS7-Base-163.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo \
    && yum clean all \
    && yum makecache

# Start
# base development tools required \
RUN yum groupinstall -y \
        "Development tools" \
    yum install -y \
        tar \
        initscripts \
        sudo \
        which \
        centos-release-scl \
        devtoolset-4-gcc-c++ \
        devtoolset-4-gcc \
        zlib \
        zlib-dev \
        openssl-devel \
        sqlite-devel \
        bzip2-devel \
        libffi-devel \
        java-1.8.0-openjdk \
        java-1.8.0-openjdk-devel \
        nss \
        asciidoc \
        rpm-build \
        rpm2cpio \
        tar \
        xmlto \
        rpmlint \
        make \
    # Install Software Collections repo (needs to be done in separate command) \
    # newer cpp 11 support required for building node modules \
    # install metron 0.7.1  \
     && curl -o /root/metron-0.7.1.tgr.gz https://m9rco-bjhb2-storage.oss-cn-beijing.aliyuncs.com/apache-metron_0.7.1-release.tar.gz \
     && tar xvf /root/metron-0.7.1.tgr.gz \
    # install python 2.7.11 but do not make it the default \
    && curl -o /usr/src/Python-2.7.11.tgz https://m9rco-bjhb2-storage.oss-cn-beijing.aliyuncs.com/Python-2.7.11.tgz \
    && cd /usr/src \
    && tar xvf Python-2.7.11.tgz \
    && rm -rf Python-2.7.11.tgz \
    && cd /usr/src/Python-2.7.11 \
    && ./configure \
    && make altinstall \
    && cd /usr/src \
    && curl -o /usr/src/setuptools-11.3.tar.gz https://m9rco-bjhb2-storage.oss-cn-beijing.aliyuncs.com/setuptools-11.3.tar.gz  \
    && tar xvf setuptools-11.3.tar.gz \
    && rm setuptools-11.3.tar.gz \
    && cd /usr/src/setuptools-11.3 \
    && python2.7 setup.py install \
    && mkdir /root/.pip \
    && echo "[global]" > /root/.pip/pip.conf \
    && echo "trusted-host=pypi.tuna.tsinghua.edu.cn" >> /root/.pip/pip.conf \
    && echo "index-url=https://pypi.tuna.tsinghua.edu.cn/simple/" >> /root/.pip/pip.conf \
    && easy_install-2.7 pip \
    # install ansible and set the configuration var \
    && pip2.7 install cryptography==2.5 ansible==2.0.0.2 boto \
    # java \
    && cd /usr/src \
    # setup maven \
    && curl -o apache-maven-3.3.9-bin.tar.gz https://m9rco-bjhb2-storage.oss-cn-beijing.aliyuncs.com/apache-maven-3.3.9-bin.tar.gz \
    && tar xzvf apache-maven-3.3.9-bin.tar.gz \
    && rm apache-maven-3.3.9-bin.tar.gz \
    && mv apache-maven-3.3.9 /opt/maven \
    && ln -s /opt/maven/bin/mvn /usr/bin/mvn \
    # install rpm tools required to build rpms \
    # create a .bashrc for root, enabling the cpp 11 toolset \
    && touch /root/.bashrc \
    && echo '/opt/rh/devtoolset-4/enable' >> /root/.bashrc \
    # install node so that the node dependencies can be packaged into the RPMs \
    && curl --silent --location https://rpm.nodesource.com/setup_6.x | bash - \
    # Remove packages just needed for builds \
    # Clean up yum caches \
    && yum clean all


# Enable systemd usage
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in ; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
    rm -f /lib/systemd/system/multi-user.target.wants/; \
    rm -f /etc/systemd/system/.wants/; \
    rm -f /lib/systemd/system/local-fs.target.wants/; \
    rm -f /lib/systemd/system/sockets.target.wants/udev; \
    rm -f /lib/systemd/system/sockets.target.wants/initctl; \
    rm -f /lib/systemd/system/basic.target.wants/; \
    rm -f /lib/systemd/system/anaconda.target.wants/*;

VOLUME ["/sys/fs/cgroup"]

CMD ["/usr/sbin/init"]
