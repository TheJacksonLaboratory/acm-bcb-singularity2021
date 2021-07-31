# Building and Modifying Containers
The following is an example of a singulaity recipie file.
I will install the base OS and then configure the tools it needs to install applications.

There are sections in the receipie file to control the build process.

### The first lines define the OS its version and pakcages. In the code here its docker layers for ubuntu
### There are no files included with this receipe.
### The post section is where applications are configured and loaded.

The appilcation we will start with here will be Samtools.

```
Bootstrap:docker
From:ubuntu
%labels
       MAINTAINER Richard Yanicky
%files
       # Files for Build Go Here
%post
       # DEBCONF Configuration
    export DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true
    echo "tzdata tzdata/Areas select US" >> /opt/preseed.txt
    echo "tzdata tzdata/Zones/US select Chicago" >> /opt/preseed.txt
    debconf-set-selections /opt/preseed.txt
    rm /opt/preseed.txt
    # Update System
       apt update && apt upgrade -y
    apt install -y wget make locales build-essential bzip2 libncurses5-dev libbz2-dev liblzma-dev zlib1g-dev
    locale-gen en_US.UTF-8
   cd /opt
    wget https://github.com/samtools/samtools/releases/download/1.12/samtools-1.12.tar.bz2
    bzip2 -d samtools-1.12.tar.bz2
    tar -xf samtools-1.12.tar
    cd samtools-1.12
    ./configure --prefix=/opt/samtools
    make
    make install
    cd /opt
    rm samtools-1.12.tar
    rm -rf samtools-1.12
%runscript
    /opt/samtools/bin/samtools
    echo "/opt/samtools/bin/samtools/"
```


This will install the OS and samtools
By default it will run samtools.

#### To modify the receipe just add the application install instructions.
In this case we are adding bcftools.

It should be added at the end of %post before the runscript section.

```
wget https://github.com/samtools/bcftools/releases/download/1.12/bcftools-1.12.tar.bz2
    bzip2 -d bcftools-1.12.tar.bz2
    tar -xf bcftools-1.12.tar
    cd bcftools-1.12
    ./configure --prefix=/opt/bcftools
    make
    make install
    cd /opt
    rm bcftools-1.12.tar
    rm -rf bcftools-1.12
```

The %runscript section will need to be modified.
We now can run either samtools or bcftools and the container will need to allow us to pass the application name.



