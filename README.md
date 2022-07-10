# Install mosesdecoder
#### Ubuntu 14.04 LTS
#### Author: Rahul Paul
#### Date: 10-July-2022

#### Run the following commands to install mosesdecoder

Install dependencies: \
sudo apt-get install g++ \
sudo apt-get install git \
sudo apt-get install subversion \
sudo apt-get install automake \
sudo apt-get install libtool \
sudo apt-get install zlib1g-dev \
sudo apt-get install libicu-dev \
sudo apt-get install libboost-all-dev \
sudo apt-get install libbz2-dev \
sudo apt-get install liblzma-dev \
sudo apt-get install python-dev \
sudo apt-get install graphviz \
sudo apt-get install imagemagick \
sudo apt-get install make \
sudo apt-get install cmake \
sudo apt-get install libgoogle-perftools-dev \
sudo apt-get install autoconf \
sudo apt-get install doxygen 

# Install Boost:-
Download: wget -O boost_1_64_0.tar.gz https://sourceforge.net/projects/boost/files/boost/1.64.0/boost_1_64_0.tar.gz/download \
unzip: tar zxvf boost_1_64_0.tar.gz \
cd boost_1_64_0 \
./bootstrap.sh \
./b2 \
After installation it will show SUCCESS. 

# Install CMPH:
Download: wget http://www.achrafothman.net/aslsmt/tools/cmph_2.0.orig.tar.gz \
unzip: tar zxvf cmph_2.0.orig.tar.gz \
cd cmph-2.0/ \
./configure \
make \
make install 

# Install XPC-RPC:
Download: wget http://www.achrafothman.net/aslsmt/tools/xmlrpc-c_1.33.14.orig.tar.gz \
unzip: tar zxvf xmlrpc-c_1.33.14.orig.tar.gz \
cd xmlrpc-c-1.33.14/ \
./configure \
make \
make install

# Install IRSTLM:
 





# Testing mosesdecoder:
cd $HOME/smt/mosesdecoder \
wget http://www.statmt.org/moses/download/sample-models.tgz \
tar xzf sample-models.tgz \
cd sample-models \
$HOME/smt/mosesdecoder/bin/moses -f $HOME/smt/mosesdecoder/phrase-model/moses.ini < $HOME/smt/mosesdecoder/phrase-model/in > out 
After running the code, output will be "it is a small house"


# Corpus Preparation
mkdir corpus \
cd corpus \
wget http://www.statmt.org/wmt13/training-parallel-nc-v8.tgz \
tar zxvf training-parallel-nc-v8.tgz

##### Tokenisation
$HOME/smt/mosesdecoder/scripts/tokenizer/tokenizer.perl -l en < $HOME/smt/mosesdecoder/corpus/training/news-commentary-v8.fr-en.en > $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.tok.en \
$HOME/smt/mosesdecoder/scripts/tokenizer/tokenizer.perl -l fr < $HOME/smt/mosesdecoder/corpus/training/news-commentary-v8.fr-en.fr > $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.tok.fr 

##### Truecasing
To get the truecasing training modelsthat are the statistics data extracted from text, type the following commands: \
$HOME/smt/mosesdecoder/scripts/recaser/train-truecaser.perl --model $HOME/smt/mosesdecoder/corpus/truecase-model.en --corpus $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.tok.en \
$HOME/smt/mosesdecoder/scripts/recaser/train-truecaser.perl --model $HOME/smt/mosesdecoder/corpus/truecase-model.fr --corpus $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.tok.fr

Above commands will generate the model files "truecase-model.en" and "truecase-model.fr" under the directory "~/corpus"

Using the extracted truecasing training models to perform the truecase function as below commands:
$HOME/smt/mosesdecoder/scripts/recaser/truecase.perl --model ~/corpus/truecase-model.en < $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.tok.en > $HOME/smt/mosesdecoder/corpus//news-commentary-v8.fr-en.true.en \
$HOME/smt/mosesdecoder/scripts/recaser/truecase.perl --model ~/corpus/truecase-model.fr < $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.tok.fr > $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.true.fr 

##### Cleaning 
$HOME/smt/mosesdecoder/scripts/training/clean-corpus-n.perl $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.true fr en $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.clean 1 80

files " ...clean.en" and "...clean.fr" will be generated in the directory "~/corpus"

cd $HOME/smt
mkdir lm
cd lm
$HOME/smt/irstlm/bin/add-start-end.sh < $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.true.en > $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.sb.en


export IRSTLM=$HOME/smt/irstlm; $HOME/smt/irstlm/bin/build-lm.sh -i $HOME/smt/mosesdecoder/corpus/news-commentary-v8.fr-en.sb.en -t ./temp -p -s improved-kneser-ney -o $HOME/smt/lm/news-commentary-v8.fr-en.lm.en

$HOME/smt/irstlm/bin/compile-lm --text=yes $HOME/smt/lm/news-commentary-v8.fr-en.lm.en.gz $HOME/smt/lm/news-commentary-v8.fr-en.arpa.en

$HOME/smt/mosesdecoder/bin/build_binary $HOME/smt/lm/news-commentary-v8.fr-en.arpa.en $HOME/smt/lm/news-commentary-v8.fr-en.blm.en

