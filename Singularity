#Bootstrap: localimage
#From: ./rivet.sif
Bootstrap: shub
From: timo-singularity/rivet

%help

  Container with Sherpa

%environment
  
  PYTHONPATH=/usr/local/lib/python3.6/site-packages:$PYTHONPATH
  export PYTHONPATH

%post

  yum -y install swig sqlite-devel

  # install lhapdf
  wget https://lhapdf.hepforge.org/downloads/?f=LHAPDF-6.3.0.tar.gz -O LHAPDF-6.3.0.tar.gz
  tar xvf LHAPDF-6.3.0.tar.gz
  rm LHAPDF-6.3.0.tar.gz
  cd LHAPDF-6.3.0
  ./configure --prefix=/usr/local
  make -j3
  make install
  
  # install Sherpa
  wget https://sherpa.hepforge.org/downloads/?f=SHERPA-MC-2.2.10.tar.gz -O SHERPA-MC-2.2.10.tar.gz
  tar xvf SHERPA-MC-2.2.10.tar.gz
  rm SHERPA-MC-2.2.10.tar.gz
  cd SHERPA-MC-2.2.10
  wget https://raw.githubusercontent.com/timo-singularity/sherpa/master/fix-memory-leak-in-python-interface.patch
  patch -p1 < fix-memory-leak-in-python-interface.patch
  ./configure --prefix=/usr/local \
    --enable-pyext \
    --enable-analysis \
    --enable-fastjet=/usr/local \
    --enable-hepmc2=/usr/local \
    --enable-lhapdf=/usr/local \
    --enable-rivet=/usr/local \
    --enable-gzip \
    --with-sqlite3=/usr
  make -j3 && make install

  ldconfig
  yum clean all
