# Singularity Basics

## Learn Singularity basics:  pull, shell, exec, search, bind mounts (25-30 min) 

#### Now that we have a brief overview of what containers are, let’s get into how we can actually interact with them.   

```
# singularity --help

Linux container platform optimized for High Performance Computing (HPC) and
Enterprise Performance Computing (EPC)

Usage:
  singularity [global options...]

Description:
  Singularity containers provide an application virtualization layer enabling
  mobility of compute via both application and environment portability. With
  Singularity one is capable of building a root file system that runs on any 
  other Linux system where Singularity is installed.

Options:
  -c, --config string   specify a configuration file (for root or
                        unprivileged installation only) (default
                        "/etc/singularity/singularity.conf")
  -d, --debug           print debugging information (highest verbosity)
  -h, --help            help for singularity
      --nocolor         print without color output (default False)
  -q, --quiet           suppress normal output
  -s, --silent          only print errors
  -v, --verbose         print additional information
      --version         version for singularity

Available Commands:
  build       Build a Singularity image
  cache       Manage the local cache
  capability  Manage Linux capabilities for users and groups
  config      Manage various singularity configuration (root user only)
  delete      Deletes requested image from the library
  exec        Run a command within a container
  help        Help about any command
  inspect     Show metadata for an image
  instance    Manage containers running as services
  key         Manage OpenPGP keys
  oci         Manage OCI containers
  overlay     Manage an EXT3 writable overlay image
  plugin      Manage Singularity plugins
  pull        Pull an image from a URI
  push        Upload image to the provided URI
  remote      Manage singularity remote endpoints, keyservers and OCI/Docker registry credentials
  run         Run the user-defined default command within a container
  run-help    Show the user-defined help for an image
  search      Search a Container Library for images
  shell       Run a shell within a container
  sif         siftool is a program for Singularity Image Format (SIF) file manipulation
  sign        Attach digital signature(s) to an image
  test        Run the user-defined tests within a container
  verify      Verify cryptographic signatures attached to an image
  version     Show the version for Singularity

Examples:
  $ singularity help <command> [<subcommand>]
  $ singularity help build
  $ singularity help instance start


For additional help or support, please visit https://www.sylabs.io/docs/
```

```
# man singularity shell

singularity(1)                                                                                               singularity(1)

NAME
       singularity-shell - Run a shell within a container

SYNOPSIS
       singularity shell [shell options...]

DESCRIPTION
       singularity shell supports the following formats:

       *.sif               Singularity Image Format (SIF). Native to Singularity 3.0+

       *.sqsh              SquashFS format.  Native to Singularity 2.4+

       *.img               ext3 format. Native to Singularity versions < 2.4.

       directory/          sandbox format. Directory containing a valid root file
                             system and optionally Singularity meta-data.

       instance://*        A local running instance of a container. (See the instance
                             command group.)

       library://*         A SIF container hosted on a Library
                             (default https://cloud.sylabs.io/library)

       docker://*          A Docker/OCI container hosted on Docker Hub or another
                             OCI registry.

       shub://*            A container hosted on Singularity Hub.

       oras://*            A SIF container hosted on an OCI registry that supports
                             the OCI Registry As Storage (ORAS) specification.

OPTIONS
       --add-caps=""      a comma separated capability list to add

       --allow-setuid[=false]      allow setuid binaries in container (root only)

       --app=""      set an application to run inside a container

       --apply-cgroups=""      apply cgroups from file for container processes (root only)

       -B,  --bind=[]       a  user-bind path specification.  spec has the format src[:dest[:opts]], where src and dest are
       outside and inside paths.  If dest is not given, it is set equal to src.  Mount options ('opts') may be specified as
       'ro'  (read-only)  or 'rw' (read/write, which is the default). Multiple bind paths can be given by a comma separated
       list.

       -e, --cleanenv[=false]      clean environment before running container

       -c, --contain[=false]      use minimal /dev and empty other directories (e.g. /tmp and  $HOME)  instead  of  sharing
       filesystems from your host

       -C, --containall[=false]      contain not only file systems, but also PID, IPC, and environment

       --disable-cache[=false]      dont use cache, and dont create cache

       --dns=""      list of DNS server separated by commas to add in resolv.conf

       --docker-login[=false]      login to a Docker Repository interactively

       --drop-caps=""      a comma separated capability list to drop

       --env=[]      pass environment variable to contained process

       --env-file=""      pass environment variables from file to contained process

       -f, --fakeroot[=false]      run container in new user namespace as uid 0

       --fusemount=[]      A FUSE filesystem mount specification of the form ': ' - where  is 'container' or 'host', speci‐
       fying where the mount will be performed ('container-daemon' or 'host-daemon' will run the  FUSE  process  detached).
       is  the path to the FUSE executable, plus options for the mount.  is the location in the container to which the FUSE
       mount will be attached. E.g. 'container:sshfs 10.0.0.1:/ /sshfs'. Implies --pid.

       -h, --help[=false]      help for shell

       -H, --home="/builddir"      a home directory specification.  spec can either be a src path or src:dest pair.  src is
       the  source  path  of the home directory outside the container and dest overrides the home directory within the con‐
       tainer.

       --hostname=""      set container hostname

       -i, --ipc[=false]      run container in a new IPC namespace

       --keep-privs[=false]      let root user keep privileges in container (root only)

       -n, --net[=false]      run container in a new network namespace (sets up a bridge network interface by default)

       --network="bridge"      specify desired network type separated by commas, each network will  bring  up  a  dedicated
       interface inside container

       --network-args=[]      specify network arguments to pass to CNI plugins

       --no-home[=false]      do NOT mount users home directory if /home is not the current working directory

       --no-init[=false]      do NOT start shim process with --pid

       --no-mount=[]      disable one or more mount xxx options set in singularity.conf

       --no-privs[=false]      drop all privileges from root user in container)

       --no-umask[=false]      do not propagate umask to the container, set default 0022 umask

       --nohttps[=false]       do  NOT use HTTPS with the docker:// transport (useful for local docker registries without a
       certificate)

       --nonet[=false]      disable VM network handling

       --nv[=false]      enable experimental Nvidia support

       -o, --overlay=[]      use an overlayFS image for persistent data storage or as read-only layer of container

       --passphrase[=false]      prompt for an encryption passphrase

       --pem-path=""      enter an path to a PEM formated RSA key for an encrypted container

       -p, --pid[=false]      run container in a new PID namespace

       --pwd=""      initial working directory for payload process inside the container

       --rocm[=false]      enable experimental Rocm support

       -S, --scratch=[]      include a scratch directory within the container that is linked to a temporary dir (use -W  to
       force location)

       --security=[]      enable security features (SELinux, Apparmor, Seccomp)

       -s, --shell=""      path to program to use for interactive shell

       --syos[=false]      execute SyOS shell

       -u, --userns[=false]      run container in a new user namespace, allowing Singularity to run completely unprivileged
       on recent kernels. This disables some features of Singularity, for example it only works with sandbox images.

       --uts[=false]      run container in a new UTS namespace

       --vm[=false]      enable VM support

       --vm-cpu="1"      number of CPU cores to allocate to Virtual Machine (implies --vm)

       --vm-err[=false]      enable attaching stderr from VM

       --vm-ip="dhcp"      IP Address to assign for container usage. Defaults to DHCP within bridge network.
       
       --vm-ram="1024"      amount of RAM in MiB to allocate to Virtual Machine (implies --vm)

       -W, --workdir=""      working directory to be used for /tmp, /var/tmp and $HOME (if -c/--contain was also used)

       -w, --writable[=false]      by default all Singularity containers are available as read only. This option makes  the
       file system accessible as read/write.

       --writable-tmpfs[=false]       makes the file system accessible as read-write with non persistent data (with overlay
       support only)

EXAMPLE
                $ singularity shell /tmp/Debian.sif
                Singularity/Debian.sif> pwd
                /home/gmk/test
                Singularity/Debian.sif> exit

                $ singularity shell -C /tmp/Debian.sif
                Singularity/Debian.sif> pwd
                /home/gmk
                Singularity/Debian.sif> ls -l
                total 0
                Singularity/Debian.sif> exit

                $ sudo singularity shell -w /tmp/Debian.sif
                $ sudo singularity shell --writable /tmp/Debian.sif

                $ singularity shell instance://my_instance

                $ singularity shell instance://my_instance
                Singularity: Invoking an interactive shell within container...
                Singularity container: > ps -ef
                UID        PID  PPID  C STIME TTY          TIME CMD
                ubuntu       1     0  0 20:00 ?        00:00:00 /usr/local/bin/singularity/bin/sinit
                ubuntu       2     0  0 20:01 pts/8    00:00:00 /bin/bash --norc
                ubuntu       3     2  0 20:02 pts/8    00:00:00 ps -ef

SEE ALSO
       singularity(1)

HISTORY
       15-Jun-2021 Auto generated by spf13/cobra

Auto generated by spf13/cobra                             Jun 2021                                           singularity(1)
```

#### How do we find a container to use?

```
# singularity search samtools 

Found 8 container images for amd64 matching "samtools":

	library://daanjg98/rnaseq/samtools:1.11

	library://icaoberg/default/samtools:v1.10,latest
		Samtools is a suite of programs for interacting with high-throughput sequencing data.
		Signed by: 5BB086918F6EAFF2EA6CAAE06DDC4BC448961610

	library://joelnulsen/default/samtools:v1

	library://kgillinder/analysis_pipelines/samtools:v1.1.12
		Signed by: b2396fa5a9d3d18ca20e82950e5ea802a07d87a3

	library://marialitovchenko/default/samtools:v.1.10

	library://summerwang/default/samtools:1.7

	library://vi.ya/rnaseq-dbs/samtools-v1.12:latest

	library://weizhu365/mocca-sv/samtools_1-9:1.0.0
```

#### Or search for the star application

```
# singularity search star

Found 12 container images for amd64 matching "star":

	library://dtrudg-sylabs/default/testarch4179:latest

	library://dtrudg-sylabs/default/testarch:bob,latest

	library://jemten/mip_containers/star:2.7.3a

	library://khodeir/default/decstar:sha256.66d9dda1a075a011537d6e8dcac7589e8e9c73989a298b1fa0519ac4e7c7e796
		Signed by: f45f0aaa38f52f285151bcb71b7a20de7d68a80c

	library://marialitovchenko/default/sra_to_bam_star:v18jan2021

	library://marialitovchenko/default/star:v.2.7.1a

	library://vi.ya/rnaseq-dbs/star_2.7.7a:latest

	library://vi.ya/rnaseq-dbs/star_2.7.7a:sha256.bef07a9d979e6150fd6f4fdca557cb38b967773eb13fcc593bd2818029140869

	library://viya/rnaseq-dbs/star-2.7.7a:latest

	library://wchoston/project1/stars2csv_env:latest

	library://yh549848/rnaseqde/star:2.6.1d

	library://yh549848/rnaseqde/star:latest,2.7.8a
  ```

#### What repo are we searching?  This is the generic Singularity basic cloud library repo where anyone can upload their containers found at https://cloud.sylabs.io/library.  (browse to this site in your browser and show how the same containers are available via this site) 

#### Pulling containers .  We are going to be working with a humorous container today called lolcow to lay the foundation for using containers.   

```
# singularity search lolcow | grep demo (this is the container we’ll pull)

library://godlovedc/demo/lolcow:latest
```
#### Let's pull the container down.

```
singularity pull library://godlovedc/demo/lolcow:latest  (disregard the WARNING message that appears)

INFO:    Downloading library image
89.2MiB / 89.2MiB [=======================================================================================] 100 % 58.6 MiB/s 0s
WARNING: integrity: signature not found for object group 1
WARNING: Skipping container verification
```

#### When we use the URI library:// we are specifying the default Singularity sylabs cloud library.  Others are available which we’ll cover in just a moment 

#### Run ls -l to show that lolcow_latest.sif was pulled or downloaded from the Singularity cloud repo. 

```
# ls -l
total 91384
-rwxrwxr-x. 1 labuser labuser 93574075 Jul 28 17:31 lolcow_latest.sif
```

#### The .sif extension stands for “singularity image format” and is the default image format singularity uses.   

#### Notice how singularity images appear as single files unlike Docker images which have multiple layers  

#### Let’s pull a container from a different source like Docker.  Let’s browse https://hub.docker.com .  Search for bamtools and select the official biocontainers/bamtools image.  Select the “Tags” link to show how you can pull various versions of the software. 

#### Run singularity pull docker://biocontainers/bamtools:v2.4.0_cv4  If you notice with this container we are pulling, Singularity is downloading multiple image source layers and converting them into a single .sif file.  (this takes a minute or 2 to complete) 

```
# singularity pull docker://biocontainers/bamtools:v2.4.0_cv4

INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
Getting image source signatures
Copying blob 34667c7e4631 done  
Copying blob d18d76a881a4 done  
Copying blob 119c7358fbfc done  
Copying blob 2aaf13f3eff0 done  
Copying blob d344a036449c done  
Copying blob 77f405472a1e done  
Copying blob 07459e7bed97 done  
Copying blob 4b62c33c231b done  
Copying blob d504e55e444c done  
Copying blob ab19a5691df7 done  
Copying blob 8740c4399f9a done  
Copying blob 374d024f102a done  
Copying blob b45a352333c1 done  
Copying blob 1e8c2c56d6ad done  
Copying blob b4ebad9eaf90 done  
Copying blob 62db1a3bbafa done  
Copying config 10d539d117 done  
Writing manifest to image destination
Storing signatures
2021/07/28 17:45:33  info unpack layer: sha256:34667c7e4631207d64c99e798aafe8ecaedcbda89fb9166203525235cc4d72b9
2021/07/28 17:45:34  warn rootless{dev/agpgart} creating empty file in place of device 10:175
2021/07/28 17:45:34  warn rootless{dev/audio} creating empty file in place of device 14:4
2021/07/28 17:45:34  warn rootless{dev/audio1} creating empty file in place of device 14:20
2021/07/28 17:45:34  warn rootless{dev/audio2} creating empty file in place of device 14:36
2021/07/28 17:45:34  warn rootless{dev/audio3} creating empty file in place of device 14:52
2021/07/28 17:45:34  warn rootless{dev/audioctl} creating empty file in place of device 14:7
2021/07/28 17:45:34  warn rootless{dev/console} creating empty file in place of device 5:1
2021/07/28 17:45:34  warn rootless{dev/dsp} creating empty file in place of device 14:3
2021/07/28 17:45:34  warn rootless{dev/dsp1} creating empty file in place of device 14:19
2021/07/28 17:45:34  warn rootless{dev/dsp2} creating empty file in place of device 14:35
2021/07/28 17:45:34  warn rootless{dev/dsp3} creating empty file in place of device 14:51
2021/07/28 17:45:34  warn rootless{dev/full} creating empty file in place of device 1:7
2021/07/28 17:45:34  warn rootless{dev/kmem} creating empty file in place of device 1:2
2021/07/28 17:45:34  warn rootless{dev/loop0} creating empty file in place of device 7:0
2021/07/28 17:45:34  warn rootless{dev/loop1} creating empty file in place of device 7:1
2021/07/28 17:45:34  warn rootless{dev/loop2} creating empty file in place of device 7:2
2021/07/28 17:45:34  warn rootless{dev/loop3} creating empty file in place of device 7:3
2021/07/28 17:45:34  warn rootless{dev/loop4} creating empty file in place of device 7:4
2021/07/28 17:45:34  warn rootless{dev/loop5} creating empty file in place of device 7:5
2021/07/28 17:45:34  warn rootless{dev/loop6} creating empty file in place of device 7:6
2021/07/28 17:45:34  warn rootless{dev/loop7} creating empty file in place of device 7:7
2021/07/28 17:45:34  warn rootless{dev/mem} creating empty file in place of device 1:1
2021/07/28 17:45:34  warn rootless{dev/midi0} creating empty file in place of device 35:0
2021/07/28 17:45:34  warn rootless{dev/midi00} creating empty file in place of device 14:2
2021/07/28 17:45:34  warn rootless{dev/midi01} creating empty file in place of device 14:18
2021/07/28 17:45:34  warn rootless{dev/midi02} creating empty file in place of device 14:34
2021/07/28 17:45:34  warn rootless{dev/midi03} creating empty file in place of device 14:50
2021/07/28 17:45:34  warn rootless{dev/midi1} creating empty file in place of device 35:1
2021/07/28 17:45:34  warn rootless{dev/midi2} creating empty file in place of device 35:2
2021/07/28 17:45:34  warn rootless{dev/midi3} creating empty file in place of device 35:3
2021/07/28 17:45:34  warn rootless{dev/mixer} creating empty file in place of device 14:0
2021/07/28 17:45:34  warn rootless{dev/mixer1} creating empty file in place of device 14:16
2021/07/28 17:45:34  warn rootless{dev/mixer2} creating empty file in place of device 14:32
2021/07/28 17:45:34  warn rootless{dev/mixer3} creating empty file in place of device 14:48
2021/07/28 17:45:34  warn rootless{dev/mpu401data} creating empty file in place of device 31:0
2021/07/28 17:45:34  warn rootless{dev/mpu401stat} creating empty file in place of device 31:1
2021/07/28 17:45:34  warn rootless{dev/null} creating empty file in place of device 1:3
2021/07/28 17:45:34  warn rootless{dev/port} creating empty file in place of device 1:4
2021/07/28 17:45:34  warn rootless{dev/ram0} creating empty file in place of device 1:0
2021/07/28 17:45:34  warn rootless{dev/ram1} creating empty file in place of device 1:1
2021/07/28 17:45:34  warn rootless{dev/ram10} creating empty file in place of device 1:10
2021/07/28 17:45:34  warn rootless{dev/ram11} creating empty file in place of device 1:11
2021/07/28 17:45:34  warn rootless{dev/ram12} creating empty file in place of device 1:12
2021/07/28 17:45:34  warn rootless{dev/ram13} creating empty file in place of device 1:13
2021/07/28 17:45:34  warn rootless{dev/ram14} creating empty file in place of device 1:14
2021/07/28 17:45:34  warn rootless{dev/ram15} creating empty file in place of device 1:15
2021/07/28 17:45:34  warn rootless{dev/ram16} creating empty file in place of device 1:16
2021/07/28 17:45:34  warn rootless{dev/ram2} creating empty file in place of device 1:2
2021/07/28 17:45:34  warn rootless{dev/ram3} creating empty file in place of device 1:3
2021/07/28 17:45:34  warn rootless{dev/ram4} creating empty file in place of device 1:4
2021/07/28 17:45:34  warn rootless{dev/ram5} creating empty file in place of device 1:5
2021/07/28 17:45:34  warn rootless{dev/ram6} creating empty file in place of device 1:6
2021/07/28 17:45:34  warn rootless{dev/ram7} creating empty file in place of device 1:7
2021/07/28 17:45:34  warn rootless{dev/ram8} creating empty file in place of device 1:8
2021/07/28 17:45:34  warn rootless{dev/ram9} creating empty file in place of device 1:9
2021/07/28 17:45:34  warn rootless{dev/random} creating empty file in place of device 1:8
2021/07/28 17:45:34  warn rootless{dev/rmidi0} creating empty file in place of device 35:64
2021/07/28 17:45:34  warn rootless{dev/rmidi1} creating empty file in place of device 35:65
2021/07/28 17:45:34  warn rootless{dev/rmidi2} creating empty file in place of device 35:66
2021/07/28 17:45:34  warn rootless{dev/rmidi3} creating empty file in place of device 35:67
2021/07/28 17:45:34  warn rootless{dev/sequencer} creating empty file in place of device 14:1
2021/07/28 17:45:34  warn rootless{dev/smpte0} creating empty file in place of device 35:128
2021/07/28 17:45:34  warn rootless{dev/smpte1} creating empty file in place of device 35:129
2021/07/28 17:45:34  warn rootless{dev/smpte2} creating empty file in place of device 35:130
2021/07/28 17:45:34  warn rootless{dev/smpte3} creating empty file in place of device 35:131
2021/07/28 17:45:34  warn rootless{dev/sndstat} creating empty file in place of device 14:6
2021/07/28 17:45:34  warn rootless{dev/tty} creating empty file in place of device 5:0
2021/07/28 17:45:34  warn rootless{dev/tty0} creating empty file in place of device 4:0
2021/07/28 17:45:34  warn rootless{dev/tty1} creating empty file in place of device 4:1
2021/07/28 17:45:34  warn rootless{dev/tty2} creating empty file in place of device 4:2
2021/07/28 17:45:34  warn rootless{dev/tty3} creating empty file in place of device 4:3
2021/07/28 17:45:34  warn rootless{dev/tty4} creating empty file in place of device 4:4
2021/07/28 17:45:34  warn rootless{dev/tty5} creating empty file in place of device 4:5
2021/07/28 17:45:34  warn rootless{dev/tty6} creating empty file in place of device 4:6
2021/07/28 17:45:34  warn rootless{dev/tty7} creating empty file in place of device 4:7
2021/07/28 17:45:34  warn rootless{dev/tty8} creating empty file in place of device 4:8
2021/07/28 17:45:34  warn rootless{dev/tty9} creating empty file in place of device 4:9
2021/07/28 17:45:34  warn rootless{dev/urandom} creating empty file in place of device 1:9
2021/07/28 17:45:34  warn rootless{dev/zero} creating empty file in place of device 1:5
2021/07/28 17:45:38  info unpack layer: sha256:d18d76a881a47e51f4210b97ebeda458767aa6a493b244b4b40bfe0b1ddd2c42
2021/07/28 17:45:38  info unpack layer: sha256:119c7358fbfc2897ed63529451df83614c694a8abbd9e960045c1b0b2dc8a4a1
2021/07/28 17:45:38  info unpack layer: sha256:2aaf13f3eff07aa25f73813096bd588e6408b514288651402aa3d0357509be7a
2021/07/28 17:45:38  info unpack layer: sha256:d344a036449c9288b1818105ae4d2abce625657573f79aa9ded51403e1a51224
2021/07/28 17:45:38  info unpack layer: sha256:77f405472a1eed27adbd29123f96da0632455b6559d2c48323f6120712d77d0d
2021/07/28 17:45:40  warn rootless{usr/bin/systemd-detect-virt} ignoring (usually) harmless EPERM on setxattr "security.capability"
2021/07/28 17:46:03  info unpack layer: sha256:07459e7bed97fc825203f1587163b670b47b9fd23f4f5946559eff752f13d184
2021/07/28 17:46:08  info unpack layer: sha256:4b62c33c231b3f2bce5d7e298d3ee0d7cbb69eed00b0943a88331cce6d092615
2021/07/28 17:46:08  info unpack layer: sha256:d504e55e444cbfaeaada3d9c8d33779ee7ac3c77d45d2a4613ec9d7e9aed603a
2021/07/28 17:46:08  info unpack layer: sha256:ab19a5691df732652cb199a12c6b8a790e2b5ec24d0fff49ec029f22fa9b3b7b
2021/07/28 17:46:08  info unpack layer: sha256:8740c4399f9a911064779ce2c4142e75d8657bc41bda574f0c984a67f225a7c2
2021/07/28 17:46:25  info unpack layer: sha256:374d024f102a3cd206a88f95a6d8920dde8566d4a48ca797c030d64d96339f87
2021/07/28 17:46:25  info unpack layer: sha256:b45a352333c1891e7bdc39efa49e50f647fdc0c8d6e5548d582f5624dd27189d
2021/07/28 17:46:25  info unpack layer: sha256:1e8c2c56d6adede53b6b8075f05bc45ba83127c8b525eba55c12bd69c126f958
2021/07/28 17:46:25  info unpack layer: sha256:b4ebad9eaf90c8ad49afcfde76da1dbea1582f2ef574c2032f91df0779683f98
2021/07/28 17:46:25  warn rootless{opt/conda/bin/python} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:25  warn rootless{opt/conda/bin/python2} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:25  warn rootless{opt/conda/lib/libffi.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:25  warn rootless{opt/conda/lib/libffi.so.6} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:25  warn rootless{opt/conda/lib/libpython2.7.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:25  warn rootless{opt/conda/lib/libsqlite3.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:25  warn rootless{opt/conda/lib/libsqlite3.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:25  warn rootless{opt/conda/lib/pkgconfig/python.pc} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:25  warn rootless{opt/conda/lib/pkgconfig/python2.pc} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:30  warn rootless{opt/conda/pkgs/libffi-3.2.1-1/lib/libffi.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:30  warn rootless{opt/conda/pkgs/libffi-3.2.1-1/lib/libffi.so.6} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:30  warn rootless{opt/conda/pkgs/python-2.7.13-0/bin/python} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:30  warn rootless{opt/conda/pkgs/python-2.7.13-0/bin/python2} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:30  warn rootless{opt/conda/pkgs/python-2.7.13-0/lib/libpython2.7.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:30  warn rootless{opt/conda/pkgs/python-2.7.13-0/lib/pkgconfig/python.pc} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:30  warn rootless{opt/conda/pkgs/python-2.7.13-0/lib/pkgconfig/python2.pc} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:31  warn rootless{opt/conda/pkgs/python-2.7.13-0/share/man/man1/python.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:31  warn rootless{opt/conda/pkgs/python-2.7.13-0/share/man/man1/python2.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:31  warn rootless{opt/conda/pkgs/sqlite-3.13.0-0/lib/libsqlite3.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:31  warn rootless{opt/conda/pkgs/sqlite-3.13.0-0/lib/libsqlite3.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:31  warn rootless{opt/conda/share/man/man1/python.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:31  warn rootless{opt/conda/share/man/man1/python2.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:31  info unpack layer: sha256:62db1a3bbafaa3d4657da60a914659aa7bfc5a579314c7059f5ce57a1698de7e
2021/07/28 17:46:31  warn rootless{opt/conda/bin/bamtools} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:31  warn rootless{opt/conda/lib/libasan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:31  warn rootless{opt/conda/lib/libasan.so.5} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libatomic.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libatomic.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libbamtools.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libgfortran.so.3} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libgomp.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libgomp.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libitm.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libitm.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/liblsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/liblsan.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libquadmath.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libquadmath.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libstdc++.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libstdc++.so.6} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libstdc++.so.6.0.21} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libtsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libtsan.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libubsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:32  warn rootless{opt/conda/lib/libubsan.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:34  warn rootless{opt/conda/pkgs/bamtools-2.4.0-3/bin/bamtools} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:34  warn rootless{opt/conda/pkgs/bamtools-2.4.0-3/lib/libbamtools.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-7.2.0-h69d50b8_2/lib/libgfortran.so.3} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-7.2.0-h69d50b8_2/lib/libstdc++.so.6.0.21} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libasan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libasan.so.5} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libatomic.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libatomic.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libgomp.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libgomp.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libitm.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libitm.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/liblsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/liblsan.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libquadmath.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libquadmath.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libtsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libtsan.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libubsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/lib/libubsan.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libasan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libasan.so.5} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libasan.so.5.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libatomic.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libatomic.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libatomic.so.1.2.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libgomp.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libgomp.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libgomp.so.1.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libitm.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libitm.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libitm.so.1.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/liblsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/liblsan.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/liblsan.so.0.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libquadmath.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libquadmath.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libquadmath.so.0.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libtsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libtsan.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libtsan.so.0.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libubsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libubsan.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libgcc-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libubsan.so.1.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libstdcxx-ng-8.2.0-hdf63c60_1/lib/libstdc++.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libstdcxx-ng-8.2.0-hdf63c60_1/lib/libstdc++.so.6} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libstdcxx-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libstdc++.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libstdcxx-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libstdc++.so.6} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/pkgs/libstdcxx-ng-8.2.0-hdf63c60_1/x86_64-conda_cos6-linux-gnu/sysroot/lib/libstdc++.so.6.0.25} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libasan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libasan.so.5} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libasan.so.5.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libatomic.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libatomic.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libatomic.so.1.2.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libgomp.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libgomp.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libgomp.so.1.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libitm.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libitm.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libitm.so.1.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/liblsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/liblsan.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/liblsan.so.0.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libquadmath.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libquadmath.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libquadmath.so.0.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libstdc++.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libstdc++.so.6} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libstdc++.so.6.0.25} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libtsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libtsan.so.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libtsan.so.0.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libubsan.so} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libubsan.so.1} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
2021/07/28 17:46:36  warn rootless{opt/conda/x86_64-conda_cos6-linux-gnu/sysroot/lib/libubsan.so.1.0.0} ignoring (usually) harmless EPERM on setxattr "user.rootlesscontainers"
INFO:    Creating SIF file...
```

#### Shelling into a container:  Now, what do we do with these containers we pulled?  First, one thing you can do with a container is interact with it 

### ***SECURITY WARNING:  We have been pulling containers someone else has built.  How do we know these containers are safe to use and no malicious content?  Answer:  you don’t.  You can assume based on the number of downloads (from Docker Hub) if a container is legitimate or not, but you can’t truly know at face value if malicious code is embedded.  Fortunately, Singularity allows rootless containers to run so you can’t cause harm to the underlying system if you run as a non-admin user.   

#### First, let’s see what OS our  Lab VM is running:  cat /etc/*release .  It looks like we are running Centos 8.4. 

```
# cat /etc/*release

CentOS Linux release 8.4.2105
NAME="CentOS Linux"
VERSION="8"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="8"
PLATFORM_ID="platform:el8"
PRETTY_NAME="CentOS Linux 8"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"
CENTOS_MANTISBT_PROJECT="CentOS-8"
CENTOS_MANTISBT_PROJECT_VERSION="8"
CentOS Linux release 8.4.2105
CentOS Linux release 8.4.2105
```

#### Now let's open a shell into our container

```
# singularity shell lolcow_latest.sif
Singularity>
```

#### If you notice our command prompt has changed and we are now in an interactive command prompt inside the container 

#### Rerun cat /etc/*release.  If you notice, we are actually in an Ubuntu 16 based container.   

```
Singularity> cat /etc/*release

DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.5 LTS"
NAME="Ubuntu"
VERSION="16.04.5 LTS (Xenial Xerus)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 16.04.5 LTS"
VERSION_ID="16.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
VERSION_CODENAME=xenial
UBUNTU_CODENAME=xenial
```

#### Run ls and notice how our /home directory and PWD is available inside the container.  The lines are blurred, even though we are in a different environment and OS, we still access some files outside the container and of course the container is sharing the host VM’s kernel/hardware. 

```
Singularity> ls
bamtools_v2.4.0_cv4.sif  lolcow_latest.sif

Singularity> pwd
/home/labuser
```

#### Run whoami and notice I am the same “labuser” as I was outside the container.  Remember, with singularity who I am outside the container is who I am inside the container.  If I want to be root in the container, I must be root outside the container.  

```
Singularity> whoami 
labuser
```

#### Run cowsay hello and notice what happens.  Cowsay is a program inside the container.  Run cowsay Welcome to this course! 

```
Singularity> cowsay hello
 _______
< hello >
 -------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

#### Run which cowsay and show how this is an application in the container and the path of it.   

```
Singularity> which cowsay
/usr/games/cowsay
```

#### Run fortune then run fortune | cowsay.  Then run fortune | cowsay | lolcat to show how you can pipe applications together 

```
Singularity> fortune 
You're definitely on their list.  The question to ask next is what list it is.

Singularity> fortune | cowsay | lolcat
 ________________________________
/ All generalizations are false, \
| including this one.            |
|                                |
\ -- Mark Twain                  /
 --------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

#### Type exit to leave the container environment 

#### Executing Containerized Commands with Exec 

```
# singularity exec lolcow_latest.sif cowsay ‘How did you get out of the container?’
 _______________________________________
< How did you get out of the container? >
 ---------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

#### Now we can bind a path from the host OS into our container. What does our host system have in the /data path and what does our container show?

```
# ls -al /data/
total 398544
drwxr-xr-x. 3 root root      4096 Jul 28 18:31 .
drwxr-xr-x. 3 root root        22 Jul  9 14:40 ..
-rwxr-xr-x. 1 root root 408084480 Jul 28 18:31 bamtools_v2.4.0_cv4.sif
-rw-r--r--. 1 root root       366 Jul 28 16:04 DATALOSS_WARNING_README.txt
drwx------. 2 root root     16384 Jul 28 16:04 lost+found
```

```
# singularity exec lolcow_latest.sif ls /data/
/bin/ls: cannot access '/data': No such file or directory
```

#### Using the --bind option allows us to make external paths available within our container.

```
# singularity exec --bind /data lolcow_latest.sif ls -al /data/
total 398544
drwxr-xr-x. 3 root    root         4096 Jul 28 18:31 .
drwxr-xr-x. 1 labuser labuser       100 Jul 28 18:36 ..
-rw-r--r--. 1 root    root          366 Jul 28 16:04 DATALOSS_WARNING_README.txt
-rwxr-xr-x. 1 root    root    408084480 Jul 28 18:31 bamtools_v2.4.0_cv4.sif
drwx------. 2 root    root        16384 Jul 28 16:04 lost+found
```
