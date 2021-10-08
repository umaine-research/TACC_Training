---
title: 'HPC at TACC'
author: L.Jackson
---
 Welcome to the Maine-eDNA Training Content
 
---------------------------------------------------
# High Performance Computing  at Texas Advanced Computing Center
<br />

:::warning
For Maine-eDNA researchers, access the [internal project website](https://sites.google.com/maine.edu/maine-edna/home?authuser=0) or the [Maine-eDNA homepage](https://umaine.edu/edna/) for additional details and information.
:::
---------------------------------------------------

[TOC]


## Account Setup

If you are interested in obtaining more information about using the services at TACC for your research needs, please contact UMaine’s [ARCSIM](https://umaine.edu/arcsim/) group at  [um.arcsim@maine.edu](mailto:um.arcsim@maine.edu).

**_If you have been contacted by your research advisor, or project coordinator about joining an existing project on TACC, please use the information provided below to get started._**

In order to use TACC systems, you must register for an account and have a cellphone or other accepted device nearby to set up Multi-Factor Authentication (MFA). 

1. Please go to [https://portal.tacc.utexas.edu/account-request](https://portal.tacc.utexas.edu/account-request) and create a new account (DO NOT check the box for PI-eligible). Then watch for the account confirmation email and click on the link in that email to confirm your identity.

2. Once you have done that, go back to the portal and login again to set up MFA. (It might take 24 hours for our system to accept the new account. If it doesn’t work the first time, don’t panic!)

3. Email Kevin Wentworth at [kevin.wentworth@umaine.edu](mailto:kevin.wentworth@umaine.edu) with your account information and he will attach your account to the UMaine allocation.

If you need assistance with HPC account setup, storage allocations, or login access please fill out the Advanced Research Computing [services request form](https://umaine.edu/arcsim/arc-services-request/).

Once your user account is attached to UMaine, you should be able to follow the user guide (see below) for the system you are on. 

:::info
Any questions regarding getting things working can be directed to TACC via the "Consulting">”Submit a Ticket” link on the user portal detailed in the help section below.
:::

## Help Desk

If you need to access support from TACC, you will need to open a consulting ticket. There is a link on the portal homepage (highlighted below), or you can click on the link [here](https://portal.tacc.utexas.edu/tacc-consulting/-/consult/tickets/create). 

![reference link](https://i.imgur.com/63xZyUV.png)


## User Guides

Please read through the user guide associated with the specific cluster you are using to run your jobs. The primary resource used by UMaine-eDNA is the Stampede2 supercomputer. 

[Stampede2 User Guide](https://portal.tacc.utexas.edu/user-guides/stampede2)

[Maverick2 User Guide](https://portal.tacc.utexas.edu/user-guides/maverick2)

[Longhorn User Guide](https://portal.tacc.utexas.edu/user-guides/longhorn)

<br />

## Connecting to TACC

The ‘ssh’ command is the standard way to connect to Stampede2. SSH is available within Linux and from the terminal app in the MAC OS. If you are using Windows, you will need an SSH client, such as [PuTTY](http://www.putty.org/), [Bitvise](http://www.bitvise.com/), [OpenSSH](http://www.openssh.com/). Initiate a login session as follows to connect to any available node on Stampede2:  

```ssh username@stampede2.tacc.utexas.edu```

To connect with X11 support (usually required for applications with graphical user interfaces):  
```ssh -X username@stampede2.tacc.utexas.edu```

To connect to a specific login node:  
```ssh username@login2.stampede2.tacc.utexas.edu```

You will then be prompted to enter your TACC password, followed by a six digit MFA security code randomly generated using the MFA pairing method you used for account setup on the TACC User Portal. To change your MFA method or for additional information, read through the documentation on [Multi-factor Authentication at TACC](https://portal.tacc.utexas.edu/tutorials/multifactor-authentication).

<br />

## Directories

There are currently three main directories on the Stampede2 cluster that you will need to know: 

![directories](https://i.imgur.com/9phrwjb.png)


1. **Home directory:** This directory has a quota of 10GB of storage and 200,000 files. This directory is backed up regularly, and not purged. Additionally, home directories are shared across nodes and have a path as follows:

	```/home/12345/username```

    Home directories can also be called by their environmental variable:

	```$HOME```
    
2. **Work Directory:** This directory has a quota of 1TB of storage. Currently there are two versions of the work directory, ```$WORK``` and ```$WORK2```. The ```$WORK``` directory is no longer in use and is now mounted as work_old to enable files to be transitioned to the updated directory ```$WORK2```. 

3. **Scratch Directory**: This directory has no storage quota. This directory is a temporary storage space. Files that have not been accessed in ten days are subject to purge. Deliberately modifying file access time (using any method, tool, or program) for the purpose of circumventing purge policies are prohibited.

<br />


## Software

TACC has a generous number of pre-installed software modules available for use. You can find a list of available software modules and versions [here](https://portal.tacc.utexas.edu/resources/software-search). I also suggest reading through the [TACC Software User Guides](https://portal.tacc.utexas.edu/software) for documentation on job scripts and instructions for building and/or running each of the software packages listed in the TACC environment.

<br />


## Running Jobs at TACC

TACC recommends that you run your jobs out of your resource's ```$SCRATCH``` file system instead of the global ```$WORK2``` file system. To run your jobs out of ```$SCRATCH```, copy (stage) the entire executable/package along with all needed job input files and/or needed libraries to your resource's ```$SCRATCH``` directory.

Compute nodes should not reference the ```$WORK``` file system unless it's to stage data in or out, and only before or after jobs. Your job script should also direct the job's output to the local scratch directory.

<br />

Below are some of the commonly used job management commands:

<table>
  <tr>
    <td><code>squeue -u username</code></td>
    <td>Check job ID, status, runtime</td>
  </tr>
  <tr>
    <td><code>scancel 012345</code></td>
    <td>Cancel job using ID</td>
  </tr>
  <tr>
    <td><code>scontrol show job=000000</code></td>
    <td>Detailed info about job configuration</td>
  </tr>
  <tr>
    <td><code>sbatch --dependency=afterok:012345 myjobscript</code></td>
    <td>Launch a job that is dependent on the completion of another job</td>
  </tr>
</table>

<br />

### Slurm Workload Manager
Use Slurm’s ```sbatch``` command to submit a batch job to one of the Stampede2 queues as follows:  ```sbatch myjobscript```

Here "myjobscript" is the name of the text file containing the ```sbatch``` directives and shell commands that describe the settings of the job you are submitting. Below is an example of a SLURM sbatch command block that is located at the beginning of your text file:

![slurm_script](https://i.imgur.com/qPQKYBV.png)

<br />

## Best Practices for Using TACC

Dozens, (sometimes hundreds) of users may be logged on at one time accessing the file systems. Think of the login nodes as a prep area, where users may edit and manage files, compile code, perform file management, issue transfers, submit new and track existing batch jobs etc. The login nodes provide an interface to the "back-end" compute nodes.

* Do not run jobs on the login node (this includes running software such as R, Python, MATLAB, etc.)

    * If you need interactive access, use the ```idev``` command or Slurm’s ```srun``` to schedule compute nodes

![](https://i.imgur.com/fqcRixX.png =500x)


* Do not run jobs in your ```$HOME``` directory. The ```$HOME``` file system is for routine file management, not routine jobs.

* Monitor your file system's quotas and usage using the ```taccinfo``` command. This output displays whenever you log on to a TACC resource. If you are close to file quota on either the ```$WORK``` or ```$HOME``` file system, your job may fail due to being unable to write output. It's important to monitor your disk and file usage on all TACC resources where you have an allocation.

* **Request Only the Resources You Need.** Make sure your job scripts request only the resources that are needed for that job. Don't ask for more time or more nodes than you really need. The scheduler will have an easier time finding a slot for a job requesting 2 nodes for 2 hours, than for a job requesting 4 nodes for 24 hours. 

* **Test your submission scripts.** Start small: make sure everything works on 2 nodes before you try 20. Work out submission bugs and kinks with 5 minute jobs that won't wait long in the queue and involve short, simple substitutes for your real workload.

* The maximum execution time for one job is **48hrs**. After this time, the job will automatically be cancelled.

* Make sure you allot enough time for your job to run before submitting to the queue. It is not possible to add resources to a job (e.g., allow more time) once you have submitted your job to the queue.

<br />

# Folder/File Permissions

Files within your main directories (```$HOME```, ```$WORK2```, ```$SCRATCH```) should be private and not viewable to others unless modified by you, the user. There are three different permission actions that can be set with a file and three different types of users: 


| Action        | User                |
| ------------- |:------------------- |
| Read ( r )    | Owner ( u )         |
| Write ( w )   | Group member ( g )  |
| Execute ( x ) | Everyone else ( o ) |


:::danger
NOTE: Each type of user can have one, or all of the permissions listed. Be sure to consider both the file permissions and the directory permissions.
:::
 
<br />

If you want to make a file writable, you need to know who should be allowed to write the file. If you want ‘anyone or everyone’, then you can simply assign the ‘ x ‘ permission to all three types of users using the ```chmod``` command, which is the standard command for making changes to permissions of files. First you specify which users you want to modify, then use the plus (+) or minus (-) to add or take away permissions. 

1. For example, if you want a file to be only executable by you, the user, input the command as follows: ```chmod u+x testfile.sh```

2. Similarly, if you want the file to be executable by everyone, you would instead use the following command to give executable permissions to all ‘a’ (owner, group, others): ```chmod a+x testfile.sh```

3. To modify the permissions of a folder and all files within it to allow all group members to read, write, and execute contents, use the following command: ```chmod -R g+rwX testfolder```

    In order to view the file permissions for everything within a particular directory, you can check the current permissions by running the following command: ```ls -ltr```  The prompt will return information regarding permissions, number of files contained within a directory, and group assigned to a given folder as follows:

<br />

![Permissions](https://i.imgur.com/EoP5yIQ.png =550x)

<br />

The ten permission flags are given in the highlighted box above. The first flag is designated with a ‘d’ to indicate a directory or ‘ - ‘ to indicate a file. The remaining nine flags, in groups of three, indicate:

![](https://i.imgur.com/R57oKU2.png =400x)

<br />

All combinations of permissions are as follows:



| Permission              | rwx |
| ----------------------- | --- |
| read, write and execute | rwx |
| read and write          | rw- |
| read and execute        | r-x |
| read only               | r-- |
| write and execute       | -wx |
| write only              | -w- |
| execute only            | --x |
| none                    | --- |

<br />

Additional things to note regarding directory permissions:

1. Users who have write permission for a directory can delete files in the directory, without having write permission for those files. 

2. Subdirectories can have less restrictive permissions than their parent directories. However, if you change directory permissions recursively ($-R), you are changing the permissions for all files and subdirectories in that directory tree.

3. You can give a user read permission for a file, but the user won’t have access to it without also having permission to traverse the directory tree that contains the file.

<br />

# Shell Scripting  

Bash users’ startup files: quick start guide found [here](https://portal.tacc.utexas.edu/tutorials/bashquickstart). This resource also contains links to a lengthy [reference manual](http://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Bash-Startup-Files) of bash commands and common syntax for shell scripting, as well as a helpful [blog post](https://blog.flowblok.id.au/2013-02/shell-startup-scripts.html) with documentation on modifying your shell environment.

There are also great lessons available through Software and Data Carpentries ([https://carpentries.org/](https://carpentries.org/)) that cover the basics of working with [command](https://datacarpentry.org/shell-genomics/)[ line for genomics](https://datacarpentry.org/shell-genomics/), and the [Unix shell](http://swcarpentry.github.io/shell-novice/) for beginners. It is recommended for new users to go through these tutorials and become familiar with using these resources. There is also a very helpful website [ExplainShell](https://explainshell.com/) that will explain the syntax of a given command. 

Below are some common bash commands to get you started.

<table>
  <tr>
    <td>Command</td>
    <td>Description</td>
  </tr>
  <tr>
    <td><code>pwd</code></td>
    <td>print working directory (current directory)</td>
  </tr>
  <tr>
    <td><code>ls</td>
    <td>list directories & files in current directory</td>
  </tr>
  <tr>
    <td><code>cd ..</td>
    <td>back up one directory</td>
  </tr>
  <tr>
    <td><code>cdw2</td>
    <td>back up to the work2 directory</td>
  </tr>
  <tr>
    <td><code>mkdir dir1</td>
    <td>makes a directory called dir1</td>
  </tr>
  <tr>
    <td><code>nano test1</td>
    <td>opens a file called test1 in editor</td>
  </tr>
  <tr>
    <td><code>rm test1</td>
    <td>removes test1 file from current directory</td>
  </tr>
  <tr>
    <td><code>rmdir dir1</td>
    <td>removes directory (dir1) only if empty</td>
  </tr>
  <tr>
    <td><code>rm -r dir1</td>
    <td>removes directory (dir1)  and all contents</td>
  </tr>
  <tr>
    <td><code>mv test1 dir2</td>
    <td>moves file (test1) into directory (dir2)</td>
  </tr>
  <tr>
    <td><code>cp test1 dir2</code></td>
    <td>copies file (test1) into directory (dir2)</td>
  </tr>
</table>

<br />



# QIIME2 Microbiome Analysis Software
![](https://i.imgur.com/Gm9nnUy.png)

---

If you are using the HPC for metabarcoding analysis of eDNA samples, TACC has a preinstalled version of QIIME2 for processing amplicon data. The current version supported by TACC is Qiime2-2021.4. It is recommended to read through the documentation on how to use the software ([https://docs.qiime2.org/2021.4/](https://docs.qiime2.org/2021.4/)). You can install and run on a conda native environment or using a virtual machine. TACC currently uses a container called singularity to run the software, if you choose to use the virtual machine install. It is recommended to read the documentation in detail for installation ([https://docs.qiime2.org/2021.4/install/](https://docs.qiime2.org/2021.4/install/)). 

Once you have installed the current version of QIIME2, the [Moving pictures tutorial](https://docs.qiime2.org/2021.4/tutorials/moving-pictures/) is a great introduction and will get you started with how to run the software using example data, before beginning with your own data. 

Submitting jobs at TACC is done using a slurm script, which sends your job (‘run’) to a queue and schedules it to run once an open node is available. You can find out more about the Slurm Workload Manager [here](https://slurm.schedmd.com/documentation.html).

<br />

:::success
***If you have any questions regarding the information contained in this guide, **please contact Laura Jackson at [laura.jackson@maine.edu](mailto:laura.jackson@maine.edu).**
:::

###### tags: `HPC` `TACC` `Training` `Metabarcoding` `Maine-eDNA`


