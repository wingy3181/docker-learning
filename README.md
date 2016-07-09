# Docker learning notes

## References for learning material

### Talks
- Dockercon 2016 - Docker for Developers - [Part I](https://www.youtube.com/watch?v=SK0sqfVn7ls) / [Part II - Docker Cloud](https://www.youtube.com/watch?v=ZsIb5tkyncA)
  - https://github.com/dgageot/dockercon16
  - https://github.com/CodeStory/lab-docker


### General
- https://docs.docker.com/engine/getstarted/
- https://docs.docker.com/engine/tutorials/
- https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/
- https://docs.docker.com/docker-for-mac/examples/
- [Understanding the architecture](https://docs.docker.com/v1.8/introduction/understanding-docker/)
- http://www.dwmkerr.com/testing-the-docker-for-mac-beta/
- http://www.ybrikman.com/writing/2015/05/19/docker-osx-dev/
- https://github.com/docker/labs/blob/master/README.md
  - https://github.com/docker/labs/blob/master/java/readme.adoc
- https://github.com/docker/labs/blob/master/slides/docker-introduction.pdf
  - https://www.youtube.com/watch?v=IgJXYU3GOM4
  - Docker Swarm image overview. See diagram on page 27

### Examples
- Docker for beginners - Learn to build an deploy your distributed applications easily to the cloud with docker  
  - http://prakhar.me/docker-curriculum/ (found out about this as part of https://github.com/docker/labs/blob/master/dockercon-us/docker-datacenter/README.md)
    - https://github.com/prakhar1989/FoodTrucks
- [Learn Docker by building a Microservice](http://www.dwmkerr.com/learn-docker-by-building-a-microservice/)
  - https://github.com/dwmkerr/node-docker-microservice
- http://blog.dubizzle.com/boilerroom/2016/08/18/setting-development-environment-docker-compose/

### Docker cloud
- https://github.com/docker/labs/blob/master/dockercon-us/docker-cloud/README.md
  - https://github.com/Cloud-Demo-Team/voting-demo.git

### Advanced (Really my TO DO)
The references below are for more advanced concepts such as using docker swarm and more ops-related activities.
- https://github.com/docker/labs/tree/master/12factor
- https://github.com/docker/labs/tree/master/dockercon-us
- https://github.com/docker/labs/tree/master/Docker-Orchestration

### 12 Factor app
- https://12factor.net/
- https://github.com/docker/labs/blob/master/12factor/README.md
- Factor 5 - Build/Release/Run
  ![Build/Release/Run](https://camo.githubusercontent.com/469d301cb2da039da20b60c2386a00005b0bda03/68747470733a2f2f646c2e64726f70626f7875736572636f6e74656e742e636f6d2f752f323333303138372f646f636b65722f6c6162732f3132666163746f722f6275696c645f72656c656173655f72756e2e706e67)

### Dockerfile
- https://www.ctl.io/developers/blog/post/dockerfile-add-vs-copy/
- http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/
- https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/

### Volumes
- http://container42.com/2014/11/03/docker-indepth-volumes/
- http://crosbymichael.com/advanced-docker-volumes.html
- http://container42.com/2013/12/16/persistent-volumes-with-docker-container-as-volume-pattern/

### IDE integration
- http://domeide.github.io/
- http://blog.dekstroza.io/osx-docker-beta-intellj-playing-nicely/

### Docker for mac
- https://forums.docker.com/t/is-it-possible-to-ssh-to-the-xhyve-machine/17426
  - `screen ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty`
  - `docker run -it --rm --name docker-for-mac-xhyve-ssh --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh`

## References
- https://docs.docker.com/engine/reference/run/ (`docker run` options reference)
- https://docs.docker.com/engine/reference/builder/ (Dockerfile reference)
- https://docs.docker.com/engine/reference/commandline/ (`docker <cmd>` options reference)
- https://docs.docker.com/machine/reference/
- https://docs.docker.com/compose/compose-file/
- https://docs.docker.com/compose/reference/overview/

## Docker for Mac vs. Docker Toolbox
- https://docs.docker.com/docker-for-mac/docker-toolbox/

**_<u>Docker for Mac</u>_**

![Docker for Mac](https://docs.docker.com/docker-for-mac/images/docker-for-mac-install.png)

![Docker architecture](https://github.com/docker/labs/raw/master/java/chapters/images/docker-architecture.png)

**_<u>Docker Toolbox</u>_**

![Docker Toolbox](https://docs.docker.com/docker-for-mac/images/toolbox-install.png)

**_Overview_**

- Installs `docker`, `docker-compose` and `docker-machine` (binaries directly to `/usr/local/bin`)
- Installs VirtualBox, Kitematic, Docker Quickstart Terminal applications
- Installs boot2docker at `/usr/local/share/boot2docker/boot2docker.iso`
  - However, `docker-machine` will use the cached version at `~/.docker/machine/cache/boot2docker.iso` and 'default' machine will use `~/.docker/machine/machines/default/boot2docker.iso` (See `toolbox/osx/mpkg/boot2dockeriso.pkg/Scripts/postinstall` for more information)
- 'Quickstart Terminal.app' invokes `/Applications/Docker/Docker Quickstart Terminal.app/Contents/Resources/Scripts/main.scpt` (See `CFBundleExecutable` key in `toolbox/osx/mpkg/quickstart.app/Contents/Info.plist` and `toolbox/osx/mpkg/quickstart.app/Contents/MacOS/applet`)
  - `~/Library/Application Support/DockerToolbox/default_terminal`
  - `/Applications/Docker/Docker Quickstart Terminal.app/Contents/Resources/Scripts/terminal.scpt`
    - `$ bash --login '/Applications/Docker/Docker Quickstart Terminal.app/Contents/Resources/Scripts/start.sh'`
_<u>NOTE: Use `Script Editor` to open the `*.scpt` files</u>_

```bash
#!/bin/bash

VM=default
DOCKER_MACHINE=/usr/local/bin/docker-machine
VBOXMANAGE=/Applications/VirtualBox.app/Contents/MacOS/VBoxManage

BLUE='\033[0;34m'
GREEN='\033[0;32m'
NC='\033[0m'

unset DYLD_LIBRARY_PATH
unset LD_LIBRARY_PATH

clear

if [ ! -f "${DOCKER_MACHINE}" ]; then
  echo "Docker Machine is not installed. Please re-run the Toolbox Installer and try again."
  exit 1
fi

if [ ! -f "${VBOXMANAGE}" ]; then
  echo "VirtualBox is not installed. Please re-run the Toolbox Installer and try again."
  exit 1
fi

"${VBOXMANAGE}" list vms | grep \""${VM}"\" &> /dev/null
VM_EXISTS_CODE=$?

if [ $VM_EXISTS_CODE -eq 1 ]; then
  "${DOCKER_MACHINE}" rm -f "${VM}" &> /dev/null
  rm -rf ~/.docker/machine/machines/"${VM}"
  #set proxy variables if they exists
  if [ -n ${HTTP_PROXY+x} ]; then
	PROXY_ENV="$PROXY_ENV --engine-env HTTP_PROXY=$HTTP_PROXY"
  fi
  if [ -n ${HTTPS_PROXY+x} ]; then
	PROXY_ENV="$PROXY_ENV --engine-env HTTPS_PROXY=$HTTPS_PROXY"
  fi
  if [ -n ${NO_PROXY+x} ]; then
	PROXY_ENV="$PROXY_ENV --engine-env NO_PROXY=$NO_PROXY"
  fi  
  "${DOCKER_MACHINE}" create -d virtualbox $PROXY_ENV --virtualbox-memory 2048 --virtualbox-disk-size 204800 "${VM}"
fi

VM_STATUS="$(${DOCKER_MACHINE} status ${VM} 2>&1)"
if [ "${VM_STATUS}" != "Running" ]; then
  "${DOCKER_MACHINE}" start "${VM}"
  yes | "${DOCKER_MACHINE}" regenerate-certs "${VM}"
fi

eval "$(${DOCKER_MACHINE} env --shell=bash ${VM})"

clear
cat << EOF


                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/


EOF
echo -e "${BLUE}docker${NC} is configured to use the ${GREEN}${VM}${NC} machine with IP ${GREEN}$(${DOCKER_MACHINE} ip ${VM})${NC}"
echo "For help getting started, check out the docs at https://docs.docker.com"
echo

USER_SHELL="$(dscl /Search -read /Users/${USER} UserShell | awk '{print $2}' | head -n 1)"
if [[ "${USER_SHELL}" == *"/bash"* ]] || [[ "${USER_SHELL}" == *"/zsh"* ]] || [[ "${USER_SHELL}" == *"/sh"* ]]; then
  "${USER_SHELL}" --login
else
  "${USER_SHELL}"
fi

```

**_Script breakdown_**

```
-> toolbox/Makefile
  * macOS *
  -> toolbox/script/build-osx
    -> toolbox/Dockerfile.osx
       - Downloads tools/utilities for building/extracting macOS packages/binaries. e.g. bomutils and xar
       - Downloads binaries for:
         - VirtualBox
         - Docker
         - Docker Machine
         - Docker Compose
         - boot2docker ISO
         - Kitematic
       - Extract the .pkg from VirtualBox and place into `/mpkg` in preparation as one of the components for the 'Docker Toolbox' image/binary
       - Add components for `/mpkg` and  update placeholder variables in metadata templates
         - `toolbox/osx/mpkg/Distribution`
         - `toolbox/osx/mpkg/docker.pkg` (packages it under `usr/local/bin`)
           - `PackageInfo`
             - install-location="/"
         - `toolbox/osx/mpkg/kitematicapp.pkg`
           - `PackageInfo`
             - install-location="/Applications/Docker/"
         - `toolbox/osx/mpkg/dockermachine.pkg` (packages it under `usr/local/bin`)
           - `PackageInfo`
             - install-location="/"
         - `toolbox/osx/mpkg/dockercompose.pkg` (packages it under `usr/local/bin`)
           - `PackageInfo`
             - install-location="/"
         - `toolbox/osx/mpkg/boot2dockeriso.pkg`
           - 'PackageInfo'
             - install-location="/usr/local/share/boot2docker"
             - <postinstall file="./postinstall"/>
               - Sets permissions on docker, docker-compose, docker-machine
               - Copies `/usr/local/share/boot2docker/boot2docker.iso` to `~/.docker/machine/cache/boot2docker.iso`
               - Stops docker engine daemon via `docker-machine stop default` command
               - Copies `/usr/local/share/boot2docker/boot2docker.iso` to `~/.docker/machine/machines/default/boot2docker.iso`
         - `toolbox/osx/mpkg/dockerquickstartterminalapp.pkg`
           - 'PackageInfo'
             - install-location="/Applications/Docker/"
             - Payload comes from `toolbox/osx/mpkg/quickstart.app`
         - `toolbox/osx/mpkg/Resources`
         - `toolbox/osx/mpkg/Plugins` (source for these plugins is located at `toolbox/osx/installerplugins`)
           - `overviewplugin.bundle` is for one of initial pages in the installation wizard
           - `quickstartplugin.bundle` is for one of the last pages in the installation wizard
           - See https://docs.docker.com/toolbox/toolbox_install_mac/ for screenshots and match with:
             - toolbox/osx/installerplugins/overviewplugin/Base.lproj/overviewplugin.xib
             - toolbox/osx/installerplugins/quickstartplugin/Base.lproj/quickstartplugin.xib
       - Re-package all of the above to `/DockerToolbox.pkg`
```

**_FAQs_**

- _Where are docker images stored?_

  - 'Docker for mac'
    >  https://forums.docker.com/t/where-does-docker-keep-images-containers-so-i-can-better-track-my-disk-usage/8370
    >  http://stackoverflow.com/questions/30604846/docker-error-no-space-left-on-device
    >  ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux

  - 'boot2docker via Docker toolbox'
    > http://stackoverflow.com/questions/19234831/where-are-docker-images-stored-on-the-host-machine
    > http://blog.thoward37.me/articles/where-are-docker-images-stored/
    > https://docs.docker.com/engine/userguide/storagedriver/aufs-driver/
    >
    >  Depends on storage driver but main longest/stable one is aufs
    >  `/var/lib/docker`

- _How to ssh into default boot2docker virtualbox image_
    > http://stackoverflow.com/questions/24286007/how-do-i-ssh-into-the-boot2docker-host-vm-that-the-vagrant-1-6-docker-provider-s
    > http://stackoverflow.com/questions/32646952/docker-machine-boot2docker-root-password
    >
    > `docker-machine ssh`
    >
    > or `ssh $(docker-machine ip)` with following username/password credentials: docker/tcuser
    >
    > sudo -i

- _How to update docker toolbox/boot2docker with newer version of https://github.com/docker/docker/releases_
  - **NOTE: Best way is to actually use the Docker toolbox binary again to update all the components (but be careful not to override docker for mac binaries)**
    > https://docs.docker.com/machine/reference/upgrade/
    > `docker-machine upgrade default`
    >
    >```
    > $ docker-machine upgrade default
    >  Waiting for SSH to be available...
    >  Detecting the provisioner...
    >  Upgrading docker...
    >  Stopping machine to do the upgrade...
    >  Upgrading machine "default"...
    >  Default Boot2Docker ISO is out-of-date, downloading the latest release...
    >  Latest release for github.com/boot2docker/boot2docker is v1.12.0-rc4
    >  Downloading /Users/cyip2/.docker/machine/cache/boot2docker.iso from https://github.com/boot2docker/boot2docker/releases/download/v1.12.0-rc4/boot2docker.iso...
    >  0%....10%....20%....30%....40%....50%....60%....70%....80%....90%....100%
    >  Copying /Users/cyip2/.docker/machine/cache/boot2docker.iso to /Users/cyip2/.docker/machine/machines/default/boot2docker.iso...
    >  Starting machine back up...
    >  (default) Check network to re-create if needed...
    >  (default) Waiting for an IP...
    >  Restarting docker...
    >```  



- _Toolbox migration for Docker for mac beta, where did it copy the images and containers from/to?_
  > - http://mgreau.com/posts/2015/08/26/copy-docker-images-from-one-docker-machine-to-another.html
  > - https://github.com/docker/toolbox/issues/68
  > - https://blog.docker.com/2015/08/docker-toolbox/
  >   - https://blog.docker.com/wp-content/uploads/2015/08/migrateboot2docker.png
  > - https://github.com/zchee/docker-machine-driver-xhyve/blob/master/xhyve/xhyve.go#L891

**_<u>Docker Toolbox and Docker for Mac coexistence</u>_**

![Docker Toolbox and Docker for Mac coexistence](https://docs.docker.com/docker-for-mac/images/docker-for-mac-and-toolbox.png)

## Docker Machine

https://docs.docker.com/machine/overview/

![Docker Machine Overview](https://docs.docker.com/machine/img/machine.png)
