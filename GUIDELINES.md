## Building the NDN Software Stack

We consider the NDN Software Stack to be composed of: 
* [ndn-cxx](https://github.com/named-data/ndn-cxx)
* [NFD](https://github.com/named-data/NFD)
* [ndn-tools](https://github.com/named-data/ndn-tools.git)
* [boost](https://github.com/boostorg/boost)
* [XRootD](https://xrootd.slac.stanford.edu/) `optional`

Bellow are a few guidelines for anyone how wants to manually install or create RPMs for NDN software stack on CentOS 7.x.

### gcc 8.2.1

Minimum GCC software version required is 5.3.0. For our purpose, we are using **gcc 8.2.1**. You can install it from [Centos Software Collections (SCL) Repository](https://wiki.centos.org/AdditionalResources/Repositories/SCL) for CentOS 7.x with the following commands:

```console
user@cms:~# yum install centos-release-scl
user@cms:~# yum install devtoolset-8-gcc*
user@cms:~# scl enable devtoolset-8 bash
```

### boost

Clone the [boost Git repo](https://github.com/boostorg/boost), build the libraries and install them:

```console
user@cms:~# git clone https://github.com/boostorg/boost.git
user@cms:~# cd boost
user@cms:~# git submodule update --init
user@cms:~# ./bootstrap.sh
user@cms:~# ./b2 -j 16
user@cms:~# ./b2 install--with=all
user@cms:~# export LD_LIBRARY_PATH=/usr/local/lib/:$LD_LIBRARY_PATH
user@cms:~# cat /usr/include/boost/version.hpp
```

### ndn-cxx
Clone the [ndn-cxx Git repo](https://github.com/named-data/ndn-cxx), compile the source files and install the libraries:

```console
user@cms:~# git clone https://github.com/named-data/ndn-cxx
user@cms:~# cd ndn-cxx
user@cms:~# git submodule update --init
user@cms:~# ./waf configure --with-examples
user@cms:~# ./waf
user@cms:~# ./waf install
user@cms:~# ldconfig
```

### NFD 

Clone the [NFD Git repo](https://github.com/named-data/NFD), compile the source files and install the libraries:

```console
user@cms:~# git clone https://github.com/named-data/NFD.git
user@cms:~# cd NFD
user@cms:~# git submodule update --init
user@cms:~# export PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig/:$PKG_CONFIG_PATH
user@cms:~# ./waf configure --with-tests
user@cms:~# ./waf
user@cms:~# ./waf install
user@cms:~# cp nfd.conf.sample /usr/local/etc/ndn/nfd.conf
```

### NDN Essential Tools

Clone the [NDN Essential Tools Git repo](https://github.com/named-data/ndn-tools.git), compile the source files and install the binaries:

```console
user@cms:~# git clone https://github.com/named-data/ndn-tools.git
user@cms:~# cd ndn-tools
user@cms:~# git submodule update --init
user@cms:~# ./waf configure --with-tests
user@cms:~# ./waf
user@cms:~# ./waf install
```

### XRootD

To install XRootD on Centos 7.x follow the [instructions](https://opensciencegrid.org/docs/data/xrootd/install-standalone/) on opensciencegrid website.

### Packaging

In order to create RPMs from you need to install the **rpmdevtools**:
```console
user@cms:~# yum install rpmdevtools
```

Setup an RPM directory tree:
```console
user@cms:~# rpmdev-setuptree
```

The above command creates *rpmbuild* directory in the current path. Inside you will find more directories. The one of interest are:
- *SOURCES*: where you need to pun your source files
- *SPECS*: where you put the spec file
- *RPMS*: where the resulted rpm will be put
- *SRPMS*: where the source code along with the spec file will be archived

After you've copied your source and spec files in the required locations, you need to go back in the parent directory of *rpmbuild* and run the following commands:
```console
user@cms:~# spectool -g -R rpmbuild/SPECS/<spec-file-name>.spec
user@cms:~# rpmbuild -v -ba rpmbuild/SPECS/<spec-file-name>.spec
```

In the end you can see the content of the RPM file:
```console
user@cms:~# rpm -qpl rpmbuild/RPMS/<arch>/<rpm-name>.rpm
```

In this repository you can find both RPMs and specs for: [boost 1.58.0](SPECS/boost-1.58.0.spec), [boost 1.69.0](SPECS/boost-1.69.0.spec), [ndn-cxx](SPECS/libndn-cxx.spec), [NFD](SPECS/nfd.spec) and [ndn-tools](SPECS/ndn-tools.spec).
