# Use an official centos7 image
FROM centos:7
LABEL authors="Dariush Najjarzade dariush@najjarza.de"
ENV TZ=Asia/Tehran
ENV LC_ALL="en_US.UTF-8"
ENV PYTHON_VERSION="3.7.0"
ENV LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/lib"

# Add additional repositorys and install required packages
RUN yum update -y; yum-builddep -y python; yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm ; yum -y --enablerepo=epel install sudo tzdata wget cowsay make gcc python-devel; yum clean all
# Install locale
RUN localedef -v -c -i en_US -f UTF-8 en_US.UTF-8 || true
# Set Timezone
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# Downloading and building python
RUN mkdir /tmp/python-build 
WORKDIR /tmp/python-build 
RUN curl https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tgz > python.tgz && tar xzf python.tgz && cd Python-$PYTHON_VERSION && ./configure --prefix=/usr/local --enable-shared && make install && WORKDIR / 
RUN rm -rf /tmp/python-build; 
RUN cowsay Timezone: $(cat /etc/timezone) ; cowsay Python Version: $(python --version) ; cowsay Pip Version: $(pip3 --version)
CMD ["/bin/bash"]

