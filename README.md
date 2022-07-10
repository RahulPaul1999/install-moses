# smt
# Ubuntu 14.04 LTS
# Author: Rahul Paul
# Date: 10-July-2022

Run the following commands to install mosesdecoder

Install dependencies:
sudo apt-get install g++ 
sudo apt-get install git
sudo apt-get install subversion
sudo apt-get install automake
sudo apt-get install libtool
sudo apt-get install zlib1g-dev
sudo apt-get install libicu-dev
sudo apt-get install libboost-all-dev
sudo apt-get install libbz2-dev
sudo apt-get install liblzma-dev
sudo apt-get install python-dev
sudo apt-get install graphviz
sudo apt-get install imagemagick
sudo apt-get install make
sudo apt-get install cmake
sudo apt-get install libgoogle-perftools-dev
sudo apt-get install autoconf
sudo apt-get install doxygen

Install Boost:-
# download: wget -O boost_1_64_0.tar.gz https://sourceforge.net/projects/boost/files/boost/1.64.0/boost_1_64_0.tar.gz/download
# unzip: tar zxvf boost_1_64_0.tar.gz 
cd boost_1_64_0
./bootstrap.sh
./b2
After installation it will show SUCCESS.

Install CMPH:
# download: wget http://www.achrafothman.net/aslsmt/tools/cmph_2.0.orig.tar.gz
# unzip: tar zxvf cmph_2.0.orig.tar.gz
cd cmph-2.0/
./configure
make
make install

Install IRSTLM:
Install XPC-RPC:
# download: wget http://www.achrafothman.net/aslsmt/tools/xmlrpc-c_1.33.14.orig.tar.gz
# unzip: tar zxvf xmlrpc-c_1.33.14.orig.tar.gz
cd xmlrpc-c-1.33.14/
./configure
make
make install
