# VM Setup
## Why a VM
* Portability - run on an external hard drive and can move from laptop to desktop
* Backups become easier - can take snapshots and jump back and forth on only the development environment
* Since I develop remotely even when developing locally it makes it easier to set up actual remote development as well
* Currently my laptops are not capable of running the VM so I just hook into the VM running on my desktop
* I prefer using Windows as my main OS due to interface (familiarity) and available tools
* However I love terminal and prefer to follow instructions with terminal - easier than following gui instructions that change frequently

## Tools
### Oracle VirtualBox
#### Configuration
* **Settings => Network => Advanced => Port Forwarding**: Host Port 3222 ==> Guest Port 22
  * All other port forwarding I do I do through SSH
* **Display => Remote Display**: Enable Server (check)
  * This is really only required for remote use
* Ram: I have 32gb on my host so I give my VM 20gb
* CPU: I have 8 Virtual Cores so I give VBox 4 of them
* Storage: I give VBox 100gb
  * Depending on the circumstance I like to split my data into multiple disks, this makes it possible to mount a disk image to another VM if I wanted to do that

### VS Code
* This runs on host machine
* This is a great tool because of the myriad of plugins (especially remote ssh)

#### Plugins
* [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
* [Remote - SSH: Editing Configuration Files](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh-edit)
* [Shebang Snippets](https://marketplace.visualstudio.com/items?itemName=rpinski.shebang-snippets)
* [Git Blame](https://marketplace.visualstudio.com/items?itemName=waderyan.gitblame)

### Remote Desktop Connection (comes with windows)
I use this when I remote into my VM from a remote laptop

## SSH Config
* Ignore **known_hosts** (in the case of having multiple vms on the same machine) == **StrictHostKeyChecking**, **UserKnownHostsFile**
* HostName, User, Port
* **Usage:** ssh vm
* **LocalForward:** Proxy traffic to the VM, this is so that if you are running servers on the VM you can just access them through localhost
* **RemoteForward:** Proxy traffic from the VM, so far this is just for android adb so I can use vysor on my host and run from my vm

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
  # The following forwards traffic from your local machine to the vm
  # So if the VM is running a server on 3000 and you hit the host's port 3000, you will get a response from the server
  # LocalForward ${HOST_INTERFACE}:${HOST_PORT} ${VM_INTERFACE}:${VM_PORT}
  LocalForward 0.0.0.0:3000 127.0.0.1:3000
  LocalForward 0.0.0.0:3001 127.0.0.1:3001
  LocalForward 0.0.0.0:3002 127.0.0.1:3002
  LocalForward 0.0.0.0:3003 127.0.0.1:3003
  LocalForward 0.0.0.0:3004 127.0.0.1:3004
  LocalForward 0.0.0.0:3005 127.0.0.1:3005
  LocalForward 0.0.0.0:3006 127.0.0.1:3006
  LocalForward 0.0.0.0:3007 127.0.0.1:3007
  LocalForward 0.0.0.0:3008 127.0.0.1:3008
  # The following forwards traffic from the remote machine to your local machine
  # So if you are running adb on your local machine on port 5037 and then you try to hit the vm's port 5037 you will get the response from the host machine
  # This is useful for android development: you can run 1. run programs like vysor from your host machine and 2. at the same time you can run an app from your vm (or remote machine)
  # RemoteForward `${HOST_INTERFACE}:${HOST_PORT} ${VM_INTERFACE}:${VM_PORT}`
  RemoteForward 127.0.0.1:5037 127.0.0.1:5037
```
## TODO
* Maybe create vagrant script for VM
* Investigate development in docker container
