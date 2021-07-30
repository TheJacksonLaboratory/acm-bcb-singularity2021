# Using Containers

There are many advantages in using containers. In the relm of scitenific computation containers facilitate scaleable acritcutres that are flexable.

Containers support many scientific disiplines and have established repositories that can be utilized. 

## Docker/Singularity Hub

https://hub.docker.com/ 
https://singularityhub.github.io/ 

Vast collection of containers that will provide solutions across:

1) Biology
2) Chemistry
3) Math/Statisics
4) Programming Languages.
...


Build a container to suit your needs.
	Include only the software and data that is needed.
        Compnenets in the os the system is built upon won't change.


Install specific software versions.
	Version of releases are important for reproducability of results.
        Stability of pipelines.


It provides integration to cloud pipelines. It is used as the core of the compuational backend of the pipeline, providning the operating system and specific alorithims to impliment. 
In either cloud or other HPC enviroements workflow managers are employed to describer and control entire workflows of data and computations

### Nextflow example

The integration for Singularity follows the same execution model implemented for Docker. You won’t need to modify your Nextflow script in order to run it with Singularity. Simply specify the Singularity image file from where the containers are started by using the -with-singularity command line option. For example:

```
nextflow run <your script> -with-singularity [singularity image file]  
```


The container will scale to the eniroment it is running in.
	# of cpu(s) available can be used without additional paramters being passed.
	Same would be true of RAM.
        Storage will be the login space with addtional storage made usable via bind.



Getting started with a application recipie file.

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
