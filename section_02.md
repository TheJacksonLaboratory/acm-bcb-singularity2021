# Singularity Basics

    ## Learn Singularity basics:  pull, shell, exec, run (./container), search, inspect, bind mounts (25-30 min, Jason) 

    - Now that we have a brief overview of what containers are, let’s get into how we can actually interact with them.   

    ```singularity --help
    
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


For additional help or support, please visit https://www.sylabs.io/docs/```

    ```man singularity shell```

    - How do we find container to use?   

    ```singularity search samtools``` 

    singularity search star to find star containers 

    what repo are we searching?  This is the generic Singularity basic cloud library repo where anyone can upload their containers found at https://cloud.sylabs.io/library.  (browse to this site in your browser and show how the same containers are available via this site) 

    Pulling containers .  We are going to be working with a humorous container today called lolcow to lay the foundation for using containers.   

    singularity search lolcow | grep godloved this is the container we’ll pull 

    singularity pull library://godlovedc/demo/lolcow:latest  (disregard the WARNING message that appears) 

    When we use the URI library:// we are specifying the default Singularity sylabs cloud library.  Others are available which we’ll cover in just a moment 

    run ls -l to show that lolcow_latest.sif was pulled or downloaded from the Singularity cloud repo. 

    .sif stands for “singularity image format” and is the default image format singularity uses.   

    notice how singularity images appear as single files unlike Docker images which have multiple layers  

    Let’s pull a container from a different source like Docker.  Let’s browse https://hub.docker.com .  Search for bamtools and select the official biocontainers/bamtools image.  Select the “Tags” link to show how you can pull various versions of the software. 

    Run singularity pull docker://biocontainers/bamtools:v2.4.0_cv4  If you notice with this container we are pulling, Singularity is downloading multiple image source layers and converting them into a single .sif file.  (this takes a minute or 2 to complete) 

    Shelling into a container:  Now, what do we do with these containers we pulled?  First, one thing you can do with a container is interact with it 

***SECURITY WARNING:  We have been pulling containers someone else has built.  How do we know these containers are safe to use and no malicious content?  Answer:  you don’t.  You can assume based on the number of downloads (from Docker Hub) if a container is legitimate or not, but you can’t truly know at face value if malicious code is embedded.  Fortunately, Singularity allows rootless containers to run so you can’t cause harm to the underlying system if you run as a non-admin user.   

    First, let’s see what OS our  Lab VM is running:  cat /etc/*release .  It looks like we are running Centos 8.4. 

    singularity shell lolcow_latest.sif .   If you notice our command prompt has changed and we are now in an interactive command prompt inside the container 

    Rerun cat /etc/*release.  If you notice, we are actually in an Ubuntu 16 based container.   

    run ls and notice how our /home directory and PWD is available inside the container.  The lines are blurred, even though we are in a different environment and OS, we still access some files outside the container and of course the container is sharing the host VM’s kernel/hardware. 

    run whoami and notice I am the same “labuser” as I was outside the container.  Remember, with singularity who I am outside the container is who I am inside the container.  If I want to be root in the container, I must be root outside the container.  

    Run cowsay hello and notice what happens.  Cowsay is a program inside the container.  Run cowsay Welcome to this course! 

    Run which cowsay and show how this is an application in the container and the path of it.   

    Run fortune then run fortune | cowsay.  Then run fortune | cowsay | lolcat to show how you can pipe applications together 

    type exit to leave the container environment 

    Executing Containerized Commands with Exec 

    singularity exec lowcow.sif cowsay ‘How did you get out of the container?’ 
