# Use Cloud VM directly from your command line

![Cloud img](https://www.padok.fr/hubfs/Images/Blog/vm_metadata.webp)

So you're a developer and using cloud instances for your projects and development purposes. It turns out you have to use your mouse or touchpad to hover and click lots of buttons to create, configure and use these cloud services. Modern problems require modern solutions.
 
Please, meet [CLVM(Cloud VM)](https://github.com/BstLabs/py-clvm). It will make your life easier.
 
CLVM is an open-source command-line tool that provides convenient access to users' cloud instances over an ssh connection.
It's built on top of [DynaCLI](https://github.com/BstLabs/py-dynacli), another awesome open-source tool by [BST Labs](https://github.com/BstLabs/).
 
## Motivation
The rationale behind this tool is to end the struggle of using the web interface to create, access, and configure several services to handle cloud instances.
 
In this article, I will try my best to show how CLVM can help to decrease the inconveniences associated with a cloud environment. Mostly I'll be writing about using AWS Instances, SSM Sessions and VSCode through CLVM.
 
 
## Installation
Install it via `pip`
```
$ pip3 install pyclvm
```
 
Thanks to DynaCLI, when installation is finished you will have the `clvm` command added to your terminal palette.
 
With a quick maneuver, just type `clvm -h` and you can read automatically derived docstrings and usage.
 
```
$ clvm -h
usage: clvm [-h] [-v] {connect,instance,redirect,ssh,ssm,vscode} ...
 
Command Line Utility to connect or redirect ports to a Cloud Virtual Machine
 
positional arguments:
 {connect,instance,redirect,ssh,ssm,vscode}
   connect             connect to a Virtual Machine
   instance            vm instance management
   redirect            start/stop port redirection session
   ssh                 start/stop ssh session
   ssm                 Session manager utilities
   vscode              Microsoft vscode utilities
 
optional arguments:
 -h, --help            show this help message and exit
 -v, --version         show program's version number and exit
```
 
## Working with instances
 
To start an instance intuitively we need to type:
```
$ clvm instance start <instance_name> <profile>
Starting <instance_name> ...
<instance_name> is running
```
Next, we connect to our running instance via `clvm connect <instance_name>` and et voilà! We can operate within our instance from the local terminal.
 
## Port redirection
In my example, I have VSCode installed on my cloud instance. So, if I use `clvm redirect start <instance_name>` I'll have my vscode server set on `http://localhost:8080/` ready.
Easy, isn't it?! That reduces the complex port redirecting process via bash scripting to the usage of only 4 consecutive statements.

## SSH key generation
The need of new ssh key generation for particular machine is met by typing `clvm ssh new <instance_name>`. This statement generates and saves the SSH key to corresponding directory with the help of AuthK under the hood. [AuthK](https://github.com/BstLabs/py-authk) is another open-source library from BST Labs. Visit the link for more detailed information and source code.

#### SSH tunneling
`clvm ssh start` command can be used for SSH tunneling. You need to specify <instance_name> as always and <port_number>.

## Session management
With the help of CLVM, we have good control over sessions.
```
$ clvm ssm session -h
usage: clvm ssm session [-h] {start,stop,ls} ...
 
positional arguments:
 {start,stop,ls}
   start          Start ssm session
   stop           Terminate ssm session
   ls             Display active sessions
 
optional arguments:
 -h, --help       show this help message and exit
 
```
With CLVM it's possible to send shell commands to cloud instance directly from your local terminal. For that purpose it has `clvm ssm shell` statement. It takes instance name as the first argument and commands in string format. To pass several commands you need to separate them with commas. For example:
```
$ clvm ssm shell <instance_name> "echo From cloud instance", "echo Hello, World!"
<instance_name> is running
From cloud instance,
Hello, World!

```

One of the good things about CLVM is that the syntax is very intuitive and easy. The help messages and commands are pretty self-explanatory.

## Using local VSCode with cloud instances
To do that we have `clvm vscode` command.
```
$ clvm vscode -h
usage: clvm vscode [-h] {install,start} ...

positional arguments:
  {install,start}
    install        install vscode local
    start          obtain token, start instance if required, and launch vscode editor

optional arguments:
  -h, --help       show this help message and exit
``` 
As it seems from the documentation, in case you don't have VSCode installed, you can use `install` to do all the installation steps automatically. You will get some useful vscode extensions installed like Python, remote-ssh, LTeX and etc.

## Summary
The article is aimed to contain information about the open-source solution for cloud developers and enthusiasts. It explains how CLVM helps to "do the thing" and not experience stress on gibberish code. The tool hides all the underlying nerdy processes and provides users with a clean and secure cloud development experience.
***
*CLVM is an open-source project maintained by [BST LABS](https://github.com/BstLabs/). Our main goal is to make organizations fully realize the extensive potential of cloud computing through a range of open source and commercial solutions. We are best known for [CAIOS](https://www.caios.io/home), the Cloud AI Operating System, a development platform featuring Infrastructure-from-Code technology. BST LABS is a software engineering unit of BlackSwan Technologies.*