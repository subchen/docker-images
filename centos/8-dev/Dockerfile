FROM centos:8

RUN yum makecache \
 && yum install -y git \
 && yum install -y make cmake autoconf automake gcc gcc-c++ libtool \
 && yum install -y openssh-clients openssl \
 && yum install -y rpm-build yum-utils createrepo \
 && yum install -y bash-completion sudo wget which zip unzip tree yajl net-tools \
 && yum install -y epel-release \
 && yum install -y sshpass jq \
 && yum clean all \
 && rm -rf /var/cache/yum/*

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
 && echo -e "LANG=en_US.UTF-8\nLC_ALL=en_US.UTF-8" > /etc/environment

################################################################
RUN yum install -y https://dl.bintray.com/subchen/yum-repo/the_silver_searcher-2.1.0-1.el8.x86_64.rpm

################################################################
ENV GOVERSION 1.14.2
ENV GOROOT /usr/share/go

ENV PATH $GOROOT/bin:$PATH

RUN curl -s -fSL https://dl.google.com/go/go${GOVERSION}.linux-amd64.tar.gz -o golang.tar.gz \
 && mkdir -p $GOROOT \
 && tar -zxf golang.tar.gz -C $GOROOT --strip-components=1 \
 && rm -f golang.tar.gz

################################################################
#ENV JAVA_VERSION 1.8.0
ENV JAVA_VERSION 11
ENV JAVA_HOME /usr/lib/jvm/java-$JAVA_VERSION-openjdk

RUN yum makecache \
 && yum install -y java-$JAVA_VERSION-openjdk-devel \
 && yum clean all \
 && rm -rf /var/cache/yum/*

################################################################
ENV MAVEN_VERSION 3.6.3
ENV MAVEN_HOME /usr/share/maven

RUN curl -s -fSL https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz -o maven.tar.gz \
 && mkdir -p $MAVEN_HOME \
 && tar xzf maven.tar.gz --strip-components=1 -C $MAVEN_HOME \
 && rm -rf maven.tar.gz \
 && ln -s $MAVEN_HOME/bin/mvn /usr/bin/mvn

#VOLUME /root/.m2/repository

################################################################
ENV NODE_VERSION 12.16.3

RUN curl -s -fSL https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz -o node.tar.gz \
 && tar -xzf node.tar.gz -C /usr/local --strip-components=1 \
 && rm -f node.tar.gz

RUN npm install -g yarn \
 && rm -rf /root/.npm

#RUN yarn global add mocha istanbul nyc spec-xunit-file js-beautify eslint \
# && rm -rf /usr/local/share/.cache/yarn

VOLUME /root/.npm

################################################################
RUN yum install -y python36 python36-devel python3-pip \
 && ln -s /usr/bin/python3 /usr/bin/python \
 && ln -s /usr/bin/pip3 /usr/bin/pip \
 && yum clean all \
 && rm -rf /var/cache/yum/*

################################################################
RUN yum install -y ruby ruby-devel rubygems rubygems-devel libffi-devel \
 && yum clean all \
 && rm -rf /var/cache/yum/*

RUN gem install fpm travis \
 && rm -rf /root/.gem \
 && mkdir -p /root/.travis \
 && echo "checked_completion: true" > /root/.travis/config.yml

################################################################
RUN yum install -y vim-enhanced \
 && yum install -y ctags cscope \
 && yum clean all \
 && rm -rf /var/cache/yum/* \
 && curl -s -fSL https://raw.githubusercontent.com/subchen/dev-box/master/vim/config/.vimrc -o /root/.vimrc \
 && curl -s -fSL https://raw.githubusercontent.com/subchen/dev-box/master/vim/update-vim-plugins.sh | bash -e

################################################################
ENV POWERLINE_VERSION 1.17.0

RUN curl -fSL https://github.com/justjanne/powerline-go/releases/download/v$POWERLINE_VERSION/powerline-go-linux-amd64 -o /usr/local/bin/powerline-go \
 && chmod +x /usr/local/bin/powerline-go

RUN mkdir -p /root/.bash \
 && curl -s -fSL https://raw.githubusercontent.com/subchen/dev-box/master/powerline-go/config/powerline-go.bash -o /root/.bash/powerline-go.bash \
 && echo "source /root/.bash/powerline-go.bash" >> /root/.bashrc

################################################################
RUN cd /usr/local/bin/ \
 && curl -fL https://getcli.jfrog.io | sh

################################################################
RUN yum install -y openssh-server \
 && yum clean all \
 && rm -rf /var/cache/yum/* \
 && ssh-keygen -q -N '' -t rsa -f /etc/ssh/ssh_host_rsa_key \
 && ssh-keygen -q -N '' -t rsa -f /etc/ssh/ssh_host_ecdsa_key \
 && ssh-keygen -q -N '' -t rsa -f /etc/ssh/ssh_host_ed25519_key \
 && sed -i '/^#*PermitRootLogin/c PermitRootLogin yes' /etc/ssh/sshd_config \
 && sed -i '/^#*UseDNS/c UseDNS=no' /etc/ssh/sshd_config \
 && rm -rf /run/nologin \
 && echo 'root:123123' | chpasswd

EXPOSE 22

#CMD [/usr/sbin/sshd, -D]

################################################################
RUN yum install -y openssh-clients \
 && yum clean all \
 && rm -rf /var/cache/yum/* \
 && mkdir -p /root/.ssh \
 && ssh-keygen -q -N '' -t rsa -f /root/.ssh/id_rsa \
 && curl -s -fSL https://raw.githubusercontent.com/subchen/dev-box/master/ssh/config/config -o /root/.ssh/config

################################################################
#VOLUME /root
