@CopyRight @jianmingfan@126.com

# pocketCloud
container-based app publish in a pocket

##### Introduction

Most people use docker to implement micro service which runs in the cloud to serve user request from web.
These requests are typically http request, sql request etc.

However, few notice people are overwhelmed by application management problem in real life.

People tends to have multiple devices of different size, running different operating systems and different applications.
This brings some difficulties in local application management. for example, I have a QQ messenger in android device, How can I
access it with its data from a Linux laptop seamlessly? (considering there is no official QQ in Linux)  or how to reintall a desktop without loosing the critical applications and the data?

VDI (virtual desktop infrastruture) solution alleviates the above problem. It publishes desktop and applications from cloud. It targets enterprise market.
Administrator grants employees with access right to specific desktops and applications. Employees launch a client, connect to company network, then start to
use application. The data generated from the application belongs to the company.

In this project, we bring VDI into a pocket. Application from different platform is packaged into docker image, stored in a mobile disk.
People insert the disk into any device(like mac, linux or windows) that supports docker engine, click the application, then application runs.
User profile and User data are separated from the application and saved in disk too. So people don't lose any data when reinstall the application.

We provide tools for end user to package the application into a docker image without the intervention from administrator.


##### Design method

      ------------         ---------------         -------------
         MAC                  Linux  PC               Win PC
      --------------       ----------------       ---------------
      docker engine        docker engine            docker engine
      --------------       ----------------       ---------------
                                 ^
                                 ^
                                 | cable or network
                                 |
            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
            ^  the docker images storage      ^
            ^                                 ^
            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Storage mounted by the host is accessed by the docker engine.
Users queries the application list from the docker engine.
User can request docker engine to start any application from the docker images

A typical docker images consists of the following layers

      ------------------
         Application
         VNC server
         Windows system
         Base image
      -----------------

Each image runs as a self-contained enviroment. Application draws into the Windows system's framebuffer.
VNC server then exports the framebuffer to VNC client.


#### Storage consideration

Docker images contains some special files like device files which requires the disk fs to be ext4-like.
Unfortunately, ext4 is not natively supported by MAC or windows.

There are some solutions to this. First is to redirect the disk usb device into the docker engine running on win/mac.
To be more detailed, the docker engine in Mac/win is shipped in a Linux virtual machine, named boot2docker. However,
boot2docker doesn't support usb device redirection currently.

Second solution is to redirect the usb disk into a vmware linux vm. In this vm, nfs export the disk out. Then in boot2docker
mount and access the disk images.

#### Auto run

When storage is attached, a script on it will auto run. The script will trigger docker engine on host to run
and load the images from the disk images.

#### Advanced topics
Implement usb device redirection for boot2docker Linux

#### Application launcher

#### Application package tool






















































