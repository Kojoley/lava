FROM i386/debian:stretch
#RUN echo deb http://deb.debian.org/debian unstable main >> /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y sudo build-essential python wget cmake gawk mlocate \
            vim llvm-dev libclang-dev libc++-dev \
            git jq bc python-minimal python-colorama zsh libjsoncpp-dev \
            libz-dev \
            python-yaml \
            python-subprocess32 python-lockfile python-sqlalchemy \
            python-psycopg2 python-pexpect python-psutil \
            python-numpy python-argparse python-pip libpq-dev


#RUN apt-get install -y -t unstable clang-3.6-dev

# replace with apt install odb libodb-dev libodb-pgsql-dev
WORKDIR /
RUN wget http://codesynthesis.com/download/odb/2.4/odb_2.4.0-1_i386.deb
RUN dpkg -i odb_2.4.0-1_i386.deb
RUN wget http://codesynthesis.com/download/odb/2.4/libodb-2.4.0.tar.gz
RUN tar xf libodb-2.4.0.tar.gz
WORKDIR /libodb-2.4.0
RUN ./configure --enable-shared && make -j $(nproc) && make install
WORKDIR /
RUN wget http://codesynthesis.com/download/odb/2.4/libodb-pgsql-2.4.0.tar.gz
RUN tar xf libodb-pgsql-2.4.0.tar.gz
WORKDIR /libodb-pgsql-2.4.0
RUN ./configure --enable-shared && make -j $(nproc) && make install

RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/usr-local-lib.conf
RUN ldconfig

RUN ln -s /usr/lib/libjsoncpp.so.0 /usr/lib/libjsoncpp.so.1
RUN updatedb

# remove on switching to distro-provided libodb
RUN echo "LIBRARY_PATH=/usr/local/lib" >> /etc/environment

# Set locale to C.UTF-8 instead of us_EN.UTF-8
RUN apt-get install -y locales
RUN locale-gen C.UTF-8
RUN locale -a
RUN update-locale LANG=C.UTF-8

# Having autoconf in the container will make building autotools packages easier
RUN apt-get install -y autoconf libtool m4 automake
