# Google-Authenticator

## chinese [Centos.7 中文安装](http://blog.yangcvo.me/2016/06/29/安全的SSH与CentOS的7谷歌身份验证双因素身份验证/)

## English CentOS 7 installation document below:

###Secure SSH with Google Authenticator Two-Factor Authentication on CentOS 7

> SSH access is always critical and you might want to find ways to improve the security of your SSH access. In this article we will see how we can secure SSH with simple two factor authentication by using Google Authenticator. Before using it you have to integrate the SSH daemon on your server with Google Authenticator one time password protocol TOTP and another restriction is that you must have your android phone with you all the time or at least the time you want SSH access. This tutorials is written for CentOS 7.

First of all we will install the open source Google Authenticator PAM module by executing the following command on the shell.

 ```
 yum install google-authenticator 
 ```
 This command will install Google authenticator on you Centos 7 Server. The next step is to get the verification code. It's a very simple command to get the verification code and scratch codes by just answering simple questions of server which he will ask you. You can do that step by running the following command:
 ```
 google-authenticator 
 ```
You will get an output like the following screenshot which is being displayed to help you step by step as this step is very important and crucial. Write down the emergency scratch codes somewhere safe, they can only be used one time each, and they're intended for use if you lose your phone.


Now download Google authenticator application on your Mobile phone, the app exists for Android and Iphone. Well I have Android so I will download it from Google Play Store where I searched it out just by typing "google authenticator".
The next step is to change some files which we will start by first changing /etc/pam.d/sshd. Add the following line to the bottom of line:
```
 auth required pam_google_authenticator.so 
 ```
 
 Change the next file which is /etc/ssh/sshd_config. Add the following line in the file and if its already placed then change the parameter to "yes":
```
 ChallengeResponseAuthentication yes 
```
 
 Now restart the service of ssh by the following command:
 ```
 service sshd restart 
 ```
Last step is to test the service by connecting with SSH to the server to see if it will require verification code. You can see the following screenshot which shows the verification code that keeps on changing time after time and you have to login with it:

![](https://www.howtoforge.com/images/ssh_two_factor_authentication/big/5.jpg)


So we have successfully configured SSH authentication based on Google Authenticator. Now your SSH is secure and no brute attack can invade your server unless someone has your verification code which will require access to your phone as well.


![](http://7xrthw.com1.z0.glb.clouddn.com/Google-OTP4.png)


如果提示找不到这个包：

I can't find the package in the standard or EPEL Centos 7 repo. please advise...

需要翻墙或者VPN才能下载：

Need to download VPN or over the wall:

```
 wget https://google-authenticator.googlecode.com/files/libpam-google-authenticator-1.0-source.tar.bz2 
 tar -xvzf libpam-google-authenticator-1.0-source.tar.bz2 
 cd libpam-google-authenticator-1.0 
 make make install
 ```
 
这里我已经下载好安装包了，你可以下载下来编译安装。


Here I have to download the installation package, you can download the compiler to install.


### matters needing attention

```
1 Selinux: need to be set to disabled
2 can be used with the RSA public key authentication, but only difference in the computer with the key RSA only, if not the words will be used.
3 with a ".Google_authenticator" can be used in Taiwan don't Server, so security must pay attention to.
4 commercial OTP system is generally C/S network version of the way, there is a unified Server Authentication, in order to ensure high availability,
Generally, there will be a host of two servers.
5 Authenticator Google is a time - based program that generates authentication code, so that it is either a server or a mobile client,
Time requirements are very strict, to maintain synchronization with the NTP server.
6 Google Authenticator and a bar code scanner is any GPRS and WIFI traffic will not default.
7 If you do not need to enter the user login OTP password, but when the user Su to the root requirements of the input,
PAM authentication can be added to the "/etc/pam.d/su".
8 when the server enables PAM authentication, all users are required to enter the TOTP password,
Therefore, each user needs to produce a ".Google_authenticator" file in its own directory.
```
