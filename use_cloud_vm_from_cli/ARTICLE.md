# Simplify Cloud Instance Management from Your command Line
*A Cloud VM (CLVM) open source tool reduces steps when working with AWS, Azure, GCP and especially VSCode Remote

![Cloud img](https://www.padok.fr/hubfs/Images/Blog/vm_metadata.webp)

So you're a developer using cloud instances for your projects and development purposes. One struggle is that you have to follow the steps below to ready your instance for use and connect to it in the most simplistic way (using AWS as an example).

1. Log in to the web interface of AWS.
2. Go to the EC2 Management Console.
3. Generate a new ssh key and add it to AWS.
4. Select your instance and start it using buttons.
5. Find your instance's user name and public DNS name.
6. Open the terminal and type `ssh -i <ssh_key_path> <user_name>@<public_dns_name>`

If you plan to use VSCode Remote or port redirection to access your cloud instance resources, then these steps are doubled.

It turns out you have to use your mouse or touchpad to hover over and click lots of buttons to start/stop instances, and configure and use these cloud services via a web interface. Modern problems require modern solutions.
 
Please meet [CLVM(Cloud VM)](https://github.com/BstLabs/py-clvm). It will make your life easier.
 
CLVM is an open-source command-line tool that provides convenient access to users' cloud instances over an SSM connection.
It's built on top of [DynaCLI](https://github.com/BstLabs/py-dynacli), another awesome open-source tool from [BST Labs](https://github.com/BstLabs/).
 
## Motivation
The rationale for this tool is to end the struggle of using the web interface to access and configure several services to handle cloud instances.
 
In this article, I will show how CLVM can help to reduce the inconveniences associated with a cloud environment. Mostly I'll be writing about using AWS, GCP, and Azure Instances, SSM Sessions and VSCode Remote through CLVM. In particular, connecting through VSCode Remote has never been this easy. Without further ado, let's dive in to the topic.

## Capabilities of CLVM
1. Instance start/stop and listing operations
2. SSH key generation and tunneling
3. Session management
4. Port redirection (forwarding)
5. VSCode Remote utilities
6. Support for AWS, GCP and, Azure
 
## Installation
Install it via `pip`
```
$ pip3 install pyclvm
```
 
Thanks to DynaCLI, when installation is finished you will have the `clvm` command added to your terminal palette.
 
With a quick maneuver, just type `clvm -h` and you can read automatically derived docstrings and usage.
 
```
$ clvm -h
usage: clvm [-h] [-v] {connect,instance,plt,redirect,ssh,ssm,vscode} ...
 
Command Line Utility to connect or redirect ports to a Cloud Virtual Machine
 
positional arguments:
 {connect,instance,redirect,ssh,ssm,vscode}
   connect             connect to a Virtual Machine
   instance            vm instance management
   plt                 change default platform (AWS, GCP, AZURE)
   redirect            start/stop port redirection session
   ssh                 start/stop ssh session
   ssm                 session manager utilities
   vscode              microsoft vscode utilities
 
optional arguments:
 -h, --help            show this help message and exit
 -v, --version         show program's version number and exit
```

## Working with instances

### SSH key generation
We need to secure the connection between two machines with a new ssh key generation. This responsibility is on CLVM as well. 
```bash
$ clvm ssh new <instance_name>
``` 
This statement generates and saves the SSH key to the corresponding directory in both of the endpoints(the local machine and the remote instance).

#### SSH tunneling
`clvm ssh start` command can be used for SSH tunneling. You need to specify <instance_name> as always and <port_number>.

The fastest route is to type `clvm connect <instance_name>`. It automatically starts the VM instance and connects to it.

To only start an instance we need to type:
```bash
$ clvm instance start <instance_name>
Starting <instance_name> ...
<instance_name> is running
```
Next, we connect to our running instance via `clvm connect <instance_name>` and et voilà! We can operate within our instance from the local terminal. Of course, you are not limited to using the only terminal. VSCode Remote and Port redirection are some of the choices. Please, suit yourself.

### Default and optional configuration arguments
Currently, CLVM supports AWS, GCP, and Azure. The following versions of the tool will work with multiple cloud platforms like GCP, Azure, etc. So optional arguments will be important to maintain these resources. Please consider that we have only 2 optional arguments with default values for now, `profile` and `8080` as the port arguments. Default values are applied unless the optional arguments are explicitly specified.

They are as follows:

`profile` = default

`8080` = 8080   #<i>port</i>

You can set it to your preferred profile or port via passing it explicitly.

Example:

```bash
$ clvm connect <instance_name> profile=<profile_name>
$ clvm redirect <instance_name> profile=<profile_name> port=<port_number>
```

### Working with VSCode Remote over SSH
You can get the most out of CLVM by using
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
As apparent in the documentation, in case you don't have VSCode installed on your local machine, you can use `install` to do all the installation steps automatically. You also will have some useful vscode extensions installed like Python, remote-ssh, LTeX, etc.

### Port redirection
In my example, I have VSCode installed on my cloud instance. So, if I use `clvm redirect start <instance_name>` I'll have my vscode server set on `http://localhost:8080/` ready.
Easy, isn't it?! That reduces the complex port redirecting process via bash scripting down to the use of only 4 consecutive statements. You don't even have to start instances separately, because the `redirect` command does that for us. Stopping redirection is a piece of cake too. Just type `stop` instead of `start` and it will exit from redirection and stop the instance as well.

### Session management
With the help of CLVM, we have good control over sessions.
```
$ clvm ssm session -h
usage: clvm ssm session [-h] {start,stop,ls} ...
 
positional arguments:
 {start,stop,ls}
   start          start ssm session
   stop           terminate ssm session
   ls             display active sessions

optional arguments:
 -h, --help       show this help message and exit
 
```
With CLVM, it's possible to send shell commands to cloud instances directly from your local terminal. For that purpose, it has a `clvm ssm shell` statement. It takes an instance name as the first argument and commands in string format. To pass several commands you need to separate them with commas. For example:
```
$ clvm ssm shell <instance_name> "echo From cloud instance", "echo Hello, World!"
<instance_name> is running
From cloud instance,
Hello, World!

```

One of the good things about CLVM is that the syntax is very intuitive and easy. The help messages and commands are pretty self-explanatory.

## Summary
The article describes a high-productivity open-source solution for cloud developers and enthusiasts. It explains how CLVM helps to "do the necessary things" and not experience stress over gibberish code. The tool hides all the underlying nerdy processes and provides users with a clean and secure cloud development experience.
***
*The author, Orkhan Shirinov, is a software developer / creator of CLVM. CLVM is an open-source project maintained by [BST LABS](https://github.com/BstLabs/). Our goal is to make organizations fully realize the extensive potential of cloud computing through a range of open source and commercial solutions. We are best known for [CAIOS](https://www.caios.io/home), a portable cloud operating system and development platform featuring Infrastructure-from-Code technology. BST LABS is a software engineering unit of [BlackSwan Technologies](https://www.blackswantechnologies.ai).*
