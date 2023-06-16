# TODO

## Before OS install

- `pkgbuild-fqdn2mac netbsd9-mac68k.pet-power-plant.local`
- boot to serial console, verbosely
- disable unneeded `qemu` devices: sound, floppy

## After OS install

### Prerequisites

```sh
# apk add nfs-utils gcc g++ procps coreutils          # Alpine
# pacman -S nfs-utils gcc inetutils                   # Arch
# apt install nfs-common gcc g++                      # Debian
# yum install nfs-utils gcc gcc-c++ redhat-lsb-core   # Red Hat
# pkg install gcc-11                                  # Solaris 11
```

### Bootstrap pkgsrc

```sh
$ mkdir ~/trees
# mount -t nfs -o bg 10.0.2.2:/Users/schmonz/trees ~schmonz/trees
# echo "also in /etc/*fstab"
# ~schmonz/trees/package-rebuild/bin/pkgbuild-bootstrap
$ cd ~/trees/dotfiles && /opt/pkg/bin/bmake dotfiles
$ mkdir -p ~/.vim && ln -s ~/trees/vimbundle ~/.vim/bundle
$ . ~/.profile
$ bmake; man bmake
$ cd ~/trees/package-rebuild/bin && bmake pkgbuild
$ cd ~/trees/pkgsrc-cvs/pkgtools/shlock && msv PKGBUILD_PLATFORM
$ make install clean
$ cd ../../security/sudo && make install clean
$ sudo pkg_admin fetch-pkg-vulnerabilities
$ cd ../../shells/bash && make install clean
$ chsh   # or passwd -e on Solaris

$ cd pkgtools/pkg_rolling-replace && mic
$ cd net/fetch && mic
$ pkgbuild-mancompress
$ cd sysutils/etckeeper && mic
$ sudo etckeeper init && sudo etckeeper commit -m 'Initial commit.'
$ ( cd /etc/pkg && sudo git remote add origin ~schmonz/trees/pkgbuild-etc.git sudo git branch -M $PLATFORM && sudo git gc && sudo git push -u origin HEAD )
$ cd meta-pkgs/pkg_developer && mic
$ pkgbuild-moretools
$ cd meta-pkgs/qmail-server && mic
$ cd www/ikiwiki && mic
$ sudo etckeeper commit -m 'My most important stuff works.'
$ cd pkgtools/pkg_comp && mic
$ sudo etckeeper commit -m 'My weekly server rebuilds might work.'
# echo "also install everything else I'm MAINTAINER for"
```


## Improvements

- Script this bootstrap process (must be easy to resume after failure)
- Start all VMs on host boot (maybe with `s6-svscan`)
- Automate a `tmux` session with windows for all of them (`pkgbuild-tmux`)
- Automate "run this command on all VMs" (to build-test packages before commit)
- Automate OS updates (`pkgbuild-osupdate`)


## Notes

- Hey, how about full `pbulk` builds?
- Set up `distcc` with build hosts at:
    - Mac Pro x86 running Ubuntu
    - Mac mini x86 running macOS
    - (if those help, awesome, get a Mac Studio)
- Alpine
    - Support `pkgtools/rc.d-boot`
    - `apk update && apk upgrade`
- Void
    - Support `pkgtools/rc.d-boot`
    - automate OS update
        - xbps-install -Su
        - vkpurge rm all