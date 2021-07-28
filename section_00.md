# Welcome and Introductions

Welcome to this tutorial on using the Singularity containerization framework to install customizable applications. 

This tutorial is a primer on how to use the Singularity containerization framework to install customizable applications and use those containers on Linux based computer systems.

Software containers and containerization technologies are emerging as a valuable research artifact to facilitate and promote scientific reproducibility.  Software containers provide a separation of the containerized application environment from the host system, acting as an abstracted virtualization layer within the operating system of the host machine.  One of the primary reasons behind this rapid adoption of containerization frameworks is that this abstracted virtualization layer allows containers to be easily migrated from one system and executed on another as long as the underlying containerization framework is present on a given system.  Another major factor driving adoption, particularly in the computational biology and bioinformatics domain, is that this abstraction and virtualization allows application and container developers to bundle up the necessary prerequisites and software dependencies required by their applications, allowing their applications to be easily transferred and executed by others seeking to utilize them on their own datasets.

This tutorial will allow participants to interact with and build Singularity containers for various computational biology and bioinformatics applications using cloud hosted resources (ex: instances in AWS, GCP, or Azure) provided to each participant for the duration of the tutorial and configured with a host system Linux operating system and Singularity containerization framework.  


## Presenters
- [William S. Sanders (Shane)](shane.sanders@jax.org) is the senior manager of the Cyberinfrastructure group at The Jackson Laboratory, where his team focuses on providing the high performance computing, storage, and other data science resources required to meet the needs of JAX faculty research efforts.  Dr. Sanders has a BS in Biochemistry and a BS in Computer Science, and obtained an MS in Computer Science and a PhD in Molecular Biology from Mississippi State University.  His research efforts focus on using artificial intelligence and high performance computing to provide insight into biological problems, in a variety of species including chicken, cotton, and rattlesnake.
- [Jason S. Macklin](jason.macklin@jax.org) is the HPC Systems Engineer at The Jackson Laboratory.  He is responsible for the design and architecture of JAX HPC systems and currently manages 7 disparate HPC resources in support of the computational needs of the JAX research efforts.  Prior to JAX, Mr. Macklin served as HPC Systems Engineer at 454 Life Sciences and Roche Life Science.  Mr. Macklin obtained a BA in English from the University of Connecticut.
- [Richard Yanicky](richard.yanicky@jax.org) is a Systems Analyst at The Jackson Laboratory, supporting the HPC and bioinformatics community through outreach and one on one project work. He holds both BS and MS degrees in Applied and Computational Mathematics from the University of Connecticut, in addition to a MS in Applied Statistics from Worcester Polytechnic Institute. He has 20 years in industry and academia supporting corporate drug discovery pipelines and biological research pipelines for companies such as Pfizer, Abbott, and Eli Lilly. Prior to JAX, Richard was most recently at UCSD supporting research in human genetics for short tandem repeats and epigenetics.
- [Aaron McDivitt](aaron.mcdivitt@jax.org) is a Systems Administrator at JAX focusing not only on maintaining the underlying storage and HPC infrastructure, but also educating and assisting the HPC user community with their research needs.  In a past life, Aaron was a math and science educator/coach at the middle and high school levels, but transitioned to the IT field in 2013.  He received his BA in Education from Cedarville University.

## Objectives
* Learn what a software container is and why adoption of containerized applications is increasing.
* Learn what existing, online resources for containers are already available (ex: DockerHub, Singularity-Hub).
* Learn how to navigate and interact with containers from the Linux command line.
* Learn how to build your own containers both using container definition files and from scratch.
* Learn how to leverage containers for your existing scientific workflows.


## Connecting to the Tutorial Resources

Please use the instructions below to connect to the Azure Lab environment for this course.

***Note:***  In order to connect to our Azure Lab environment, you must sign-in with a Microsoft account.  This could be your organizational Microsoft/Office 365 email account, a personal Microsoft account, or if you do not have one feel free to [create a new Microsoft account.]( https://signup.live.com/signup?lcid=1033&wa=wsignin1.0&rpsnv=13&ct=1626965616&rver=7.0.6738.0&wp=MBI_SSL&wreply=https%3a%2f%2faccount.microsoft.com%2fauth%2fcomplete-signin%3fru%3dhttps%253A%252F%252Faccount.microsoft.com%252F%253Frefp%253Dsignedout-index%2526refd%253Dwww.google.com&lc=1033&id=292666&lw=1&fl=easi2&mkt=en-US&lic=1&uaid=b86e42f7131342ba8c0b54d0668262f5)

Click [Here](https://github.com/TheJacksonLaboratory/acm-bcb-singularity2021/blob/main/ACM-BCB%20user%20Lab%20pdf.pdf) to view a step-by-step guide with images. 

**1.** [Connect to the Azure Lab enviornment](https://labs.azure.com/register/s9faxsnt)


**2.** Sign-in with your institutional Microsoft/Office 365 account or personal Microsoft account

![image](https://user-images.githubusercontent.com/49786676/126809695-c359fa1a-404c-48a4-a787-6c539561c0e8.png)

**3.** Your VM should already be running.  Click the computer icon in the lab preview box.  Your custom ssh access command will be visible.  

![image](https://user-images.githubusercontent.com/49786676/126790061-e91a5e18-ff0e-4b9b-bbeb-0ee2a5982f88.png)


***Mac Users only!!!***:  Click “Copy” to save the command to your clipboard.

![image](https://user-images.githubusercontent.com/49786676/126809991-8d2263c8-4d92-45eb-bdf6-8f9a91523f5e.png)

***Windows Users***:  Manually copy the highlighted portion below.  Take note also of the 5 digit port number listed after the -p flag.

![image](https://user-images.githubusercontent.com/49786676/126810201-bc3b844c-9cbe-42ad-91ee-110a28ee97e9.png)

4. Connect to your VM from you terminal

***Mac Users***: Open the Terminal app (Applications>Utilities>Terminal) or your preferred terminal app of choice on your Mac and paste in the command you copied previously.  If prompted, type "yes" to accept the host ID.  

Use the password **acmBCB2021** to sign into your virtual machine

![image](https://user-images.githubusercontent.com/49786676/126811176-513d594f-e480-472e-a1ae-aacf6bb5c0b9.png)

***Windows Users***:  Using an app such as [Putty](https://www.putty.org/) or [MobaXterm](https://mobaxterm.mobatek.net/download.html), paste in the previously copied *labuser@ml-lab-de...* hostname address in the Host Name box ensuring the connection type is SSH.  Put in the 5 digit port number from the previous step.  This likely will be a different number than the image below.  Click Open to connect.

If prompted, click "Accept" to accept the host ID.

Use the password **acmBCB2021** to sign into your virtual machine

![image](https://user-images.githubusercontent.com/49786676/126813902-9cb21304-a2f4-4c85-9d7d-965cd5a06104.png)

![image](https://user-images.githubusercontent.com/49786676/126814016-018dd200-6e80-408e-a3a3-d6841f7c977f.png)



