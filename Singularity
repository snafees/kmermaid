Bootstrap: docker
From: continuumio/anaconda3
%files
docker/sysctl.conf /etc/sysctl.conf
%labels
MAINTAINER olga.botvinnik@czbiohub.org
org.label-schema.vcs-ref=$VCS_REF 
org.label-schema.vcs-url="e.g. https://github.com/czbiohub/nf-kmer-similarity"
%post

# Suggested tags from https://microbadger.com/labels


cd /home

USER root

# Add user "main" because that's what is expected by this image
useradd -ms /bin/bash main


PACKAGES=zlib1g
git=g++
make=ca-certificates
gcc=zlib1g-dev
libc6-dev=procps

### don't modify things below here for version updates etc.

cd /home

apt-get update && \
apt-get install -y --no-install-recommends ${PACKAGES} && \
apt-get clean

conda install --yes Cython bz2file pytest numpy matplotlib scipy sphinx alabaster

cd /home && \
git clone https://github.com/dib-lab/khmer.git -b master && \
cd khmer && \
python3 setup.py install

# Check that khmer was installed properly
trim-low-abund.py --help
trim-low-abund.py --version

conda install --channel bioconda --yes sourmash

# Required for multiprocessing of 10x bam file
# RUN pip install pathos bamnostic

# ENV SOURMASH_VERSION master
cd /home && \
git clone https://github.com/dib-lab/sourmash.git && \
cd sourmash && \
python3 setup.py install

which -a python3
python3 --version
sourmash info

USER main
%environment
export PACKAGES=zlib1g
export git=g++
export make=ca-certificates
export gcc=zlib1g-dev
export libc6-dev=procps
%runscript
exec /bin/bash "$@"
