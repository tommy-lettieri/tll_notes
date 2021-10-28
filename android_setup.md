# Android Setup

## Tools
### Android Studio (VM)
* [Download](https://developer.android.com/studio)

### Android SDK Platform Tools (Host)
* [Download](https://developer.android.com/studio/releases/platform-tools)
#### Setup
* Since these are portable I create a "Portable" folder in the c drive and put them in there
* I then make a sym link in ~/mypath (see bash_setup)
    * **Git Bash:** `~/mypath/adb`
        * Script
            ```bash
            #!/usr/bin/env bash
            /c/Portable/platform-tools_r31.0.3-windows/platform-tools/adb.exe $@
            ```
    * **MobaXTerm:** `ln -s /mnt/c/Portable/platform-tools_r31.0.3-windows/platform-tools/adb.exe ~/mypath/adb`


### Vysor (**Host** or VM)
* Vysor is a tool for mirroring your Android device on your local machine
* I install this on my host machine since I use Vysor outside of development, however it does have a linux and browser dist
* Plus it should be more efficient on local machine (granted you can say that about the whole setup, especially when you don't have remote ssh features - which is the case for android)
* [Download](https://www.vysor.io/download/)

### RDP (Host)
* As mentioned in vm_setup, I turn on remote desktop in virtualbox and use windows remote desktop connection to remote into the vm
* IntelliJ / Android Studio do not have remote ssh features... yet

## Applicable SSH Config (for remote development)
* Config file location: `~/.ssh/config`
* Forward vm adb traffic from host
* Ignore known hosts
* Connection info

This configuration is required because 
```bash
Host vm
  HostName localhost
  Port 3222
  User THE_USER
  # The next two keys are a bit of a security vulnerability... And are only required if you have multiple vms running on the same host (for instance a work vm and a personal one)
  # This is fine if you know the host is safe... especially if the host is localhost
  # Don't prompt user to trust source
  StrictHostKeyChecking no
  # If the host is already in known hosts don't block
  UserKnownHostsFile /dev/null
  # The following forwards traffic from the remote machine to your local machine
  # So if you are running adb on your local machine on port 5037 and then you try to hit the vm's port 5037 you will get the response from the host machine
  # This is useful for android development: you can run 1. run programs like vysor from your host machine and 2. at the same time you can run an app from your vm (or remote machine)
  # RemoteForward `${HOST_INTERFACE}:${HOST_PORT} ${VM_INTERFACE}:${VM_PORT}`
  RemoteForward 127.0.0.1:5037 127.0.0.1:5037
```