# Node Setup
## Tools
### VSCode
#### Plugins
* [DotENV](https://marketplace.visualstudio.com/items?itemName=mikestead.dotenv)
* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

# SSH Config
* Forward local traffic to remote vm
* Ignore known hosts
* Connection info

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
```