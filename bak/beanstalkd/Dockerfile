FROM debian:jessie
 

RUN apt-get update && apt-get install -y curl build-essential

# download
RUN curl -sL https://github.com/kr/beanstalkd/archive/v1.10.tar.gz | tar xvz -C /tmp

# build and install
RUN cd /tmp/beanstalkd-1.10 && make && make install
# RUN cp beanstalkd /usr/bin

EXPOSE 11300
CMD ["beanstalkd", "-p", "11300"]