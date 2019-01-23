#NodeJS Redis with SSH
#Auther rc@sarv.com

FROM node:carbon

RUN wget http://download.redis.io/redis-stable.tar.gz && \
    tar xvzf redis-stable.tar.gz && \
    cd redis-stable && \
    make && \
    mv src/redis-server /usr/bin/ && \
    cd .. && \
    rm -r redis-stable && \
    npm install -g concurrently


RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo 'root:PassWord@jdkls88' | chpasswd
RUN echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
#RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN echo 'ClientAliveInterval 300' >> /etc/ssh/sshd_config
RUN echo  'ClientAliveCountMax 10' >> /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile


EXPOSE 22
EXPOSE 6379

WORKDIR /app


RUN apt-get -y install vim

# RUN npm install

COPY app /app


CMD concurrently "/usr/bin/redis-server --bind '0.0.0.0'" "sleep 5s; node /app/index.js" "/usr/sbin/sshd"
