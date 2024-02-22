# centos7-install-glibc2.28
The provided set of instructions outlines a comprehensive process to upgrade key development tools and the GNU C Library (glibc) on a CentOS 7 system to version 2.28. The steps involve installing essential dependencies, upgrading the make utility, updating the GCC compiler, and finally, upgrading the GNU C Library.

### Install Base Dependencies
```
yum install -y epel-release
yum install -y bison wget bzip2 gcc gcc-c++ glibc-headers
```

### Upgrade make
```
# Download and extract make source
wget http://ftp.gnu.org/gnu/make/make-4.2.1.tar.gz
tar -zxvf make-4.2.1.tar.gz
cd make-4.2.1

# Build and install make
mkdir build
cd build
../configure --prefix=/usr/local/make && make && make install

# Update PATH and create a symbolic link for compatibility
export PATH=/usr/local/make/bin:$PATH
ln -s /usr/local/make/bin/make /usr/local/make/bin/gmake

# Verify the updated make version
make -v
```

### Install GCC from DevToolset 8
```
# Install Software Collections (SCL) repository
yum install -y centos-release-scl

# Install GCC and related tools from DevToolset 8
yum install -y devtoolset-8-gcc devtoolset-8-gcc-c++ devtoolset-8-binutils

# Enable DevToolset 8
echo "source /opt/rh/devtoolset-8/enable" >> /etc/profile
source /etc/profile
```

### Upgrade GLIBC
```
# Download and extract glibc source
wget https://ftp.gnu.org/gnu/glibc/glibc-2.28.tar.xz
tar -xvf glibc-2.28.tar.xz
cd glibc-2.28

# Build and install glibc
mkdir build
cd build
../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
make -j4
make install

# Resolve encoding issues and install localization data
make localedata/install-locales
```

### Check GLIBC Version
```
# Verify the installed GLIBC version
strings /lib64/libc.so.6 | grep ^GLIBC_
```
