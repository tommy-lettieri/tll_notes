# Bash Setup
* The following two bullets make it easier for me to view my own customizations without
* If I want to add to one of these files and it already exists I create a new file and then source it
  * `.bashrc`
    ```bash
    source ~/.my_bashrc.sh
    ```
  * `.my_bashrc.sh`
    ```bash
    #!/usr/bin/env bash
    echo "RUNNING .my_bashrc.sh $(pwd)"
    PATH=$PATH:~/mypath
    ```
* Instead of playing with bin folders I create a folder `~/mypath` and then add it to the path
  * Commands
  ```bash
  mkdir ~/mypath
  echo 'PATH=$PATH:~/mypath' >> ~/.my_bashrc.sh
  ```

## Windows Caveats
### Git Bash
* I use git bash and I know there are sym link configurations, but I had some problems so I gave up and just use a script instead
  * Example:
  `adb`
  ```bash
  #!/usr/bin/env bash
  /c/Portable/platform-tools_r31.0.3-windows/platform-tools/adb.exe $@
  ```
### Mobaxterm
* This did not give me any issues with sym links