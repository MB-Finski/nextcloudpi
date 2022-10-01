# Containerize NCP

_A project that started from a brainstorming in the Matrix Wiki chat room._  
First post & preliminary information and all that sort of stuff :pray: 

A very much Work In Progress, confirming and testing if a design idea could indeed work as thought-out theoretically

## Project idea 

Convert NCP and it's tool into something like a "binary" application container (or containers that only do "one thing/task") and services capable of being integrated with others, also making it possible to update/upgrade parts of the whole instead of everything.

Where ncp-config is the master container over the others, and this image can then be used as a service. 

## End goal
 
Containerize NCP completely

## Starting point & proof-of-concept

Convert `ncp-config`'s various scripts into individual containers & `ncp-config` to a container as well, being used as the master container, to control the others. 

- [ ] [NCP-Config][ncp-config]   _Check me when the conversion is complete._
  - [ ] Category re-design/re-structuring

- [ ] New category suggestion
  - BACKUP
  - NETWORK
  - SYSTEM
  - UPDATE

TODO
  1. - [X] Added a few relevant help articles, for basic understanding around the subject of the project.
  2. - [X] Added some more relevant help articles from the Docker documentation, can be really hard to find otherwise.
  3. - [x] Add links and script names to the list until completed
      1.  - [X] [BACKUPS][dirBackups] 
      4.  - [X] [CONFIG][dirConfig]  
      5.  - [x] [NETWORKING][dirNetworking]
      6.  - [x] [SECURITY][dirSecurity]
      7.  - [x] [SYSTEM][dirSystem]
      8.  - [x] [TOOLS][dirTools]
      9.  - [x] [UPDATES][dirUpdates]
  2. - [ ] Expand explanations 
  3. - [ ] Begin research & testing
  4. - [ ] What else? ..

<!-- START Master scripts links -->
[ncp-config]: https://github.com/nextcloud/nextcloudpi/blob/master/bin/ncp-config
<!-- END Master scripts links -->

## Related Help articles & Documentation information

[Google - Best practice, Building containers][1]  
[Google - Best practice, Operating containers][2]  
[Docker - Best practice, Dockerfile][3]  
[Docker - Best practice, Development][4]  
[Docker - Best practice, Image-building][9]  
[Docker - Build enhancements][5]  
[Docker - Choosing a build driver][6]  
[Docker - Manage images][7]  
[Docker - Create a base image][8]  

[Docker - Multi-container apps][10]  
[Docker - Update the application][11]  
[Docker - Packaging your software][12]  
[Docker - Multi-stage builds][13]  
[Docker - Compose, Overview][14]  
[Docker - Reference, run command][15]  
[Docker - Specify a Dockerfile][18]  

[Docker - Announcement, Compose V2][16]

[Red Hat Dev - Blog Post, Systemd in Containers][17]

[Docker docs, Deprecated Features][docker-deprecated]

<!-- START Help articles -->
[1]: https://cloud.google.com/architecture/best-practices-for-building-containers#signal-handling
[2]: https://cloud.google.com/architecture/best-practices-for-operating-containers
[3]: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
[4]: https://docs.docker.com/develop/dev-best-practices/
[5]: https://docs.docker.com/develop/develop-images/build_enhancements/
[6]: https://docs.docker.com/build/building/drivers/
[7]: https://docs.docker.com/develop/develop-images/image_management/
[8]: https://docs.docker.com/develop/develop-images/baseimages/
[9]: https://docs.docker.com/get-started/09_image_best/
[10]: https://docs.docker.com/get-started/07_multi_container/
[11]: https://docs.docker.com/get-started/03_updating_app/
[12]: https://docs.docker.com/build/building/packaging/
[13]: https://docs.docker.com/build/building/multi-stage/
[14]: https://docs.docker.com/compose/
[15]: https://docs.docker.com/engine/reference/run
[16]: https://www.docker.com/blog/announcing-compose-v2-general-availability/
[17]: https://developers.redhat.com/blog/2019/04/24/how-to-run-systemd-in-a-container
[18]: https://docs.docker.com/engine/reference/commandline/build/#specify-a-dockerfile--f
[docker-deprecated]: https://docs.docker.com/engine/deprecated/
<!-- END Help articles -->

## Notes

[Gist Notes, Docker Commands][docker-cmd]

[Docker Hub, Nextcloudpi][ncp-docker-hub]

<!-- START Notes Links --> 
[docker-cmd]: https://gist.github.com/ZendaiOwl/f80b09d792d3b7ed51eb00e72ab866de
[ncp-docker-hub]: https://hub.docker.com/r/ownyourbits/nextcloudpi
[docker-orchestration]: https://docs.docker.com/get-started/orchestration/
<!-- END Notes Links -->

#### Docker Context

[Docker docs, Manage contexts](https://docs.docker.com/engine/reference/commandline/context/)

#### Docker Buildx

- `docker buildx build . -f /path/Dockerfile --tag ${OWNER}/${REPO}:${TAG}`

Options

- `--platform`
- `--builder`
- `--push`
- `--build-arg`

Create builder

- `docker buildx create --use --name container --driver docker-container`

Drivers

- `docker`
- `docker-container` _recommended for multiple architecture compatibility_
- `kubernetes` _recommended for simultaneous multiple architecture build, one node per architecture in the cluster, combine with docker-container driver_

[Orchestration][docker-orchestration]

- `Docker Swarm`
- `Kubernetes`

#### Docker Compose 

[Docker docs, Compose extend services][docker-extend-services]  
[Docker docs, Compose networking][compose-networking]  
[Docker docs, Compose in production][compose-production]  
[Docker docs, Compose V2 compatibility][compose-v2-compat]  
[Docker docs, Compose FAQ][compose-faq]  

<!-- START Compose Links -->
[docker-extend-services]: https://docs.docker.com/compose/extends/
[compose-networking]: https://docs.docker.com/compose/networking/
[compose-production]: https://docs.docker.com/compose/production/
[compose-v2-compat]: https://docs.docker.com/compose/cli-command-compatibility/
[compose-faq]: https://docs.docker.com/compose/faq/
<!-- END Compose Links -->

Old syntax - V1  

- `docker-compose`

New syntax - V2  

- `docker compose`

File `docker-compose.yml`

```yaml
services:
  nextcloudpi:
    command: "$(ip addr | grep 192 | awk '{print $2}' | cut -b 1-14)"
    container_name: nextcloudpi
    image: ownyourbits/nextcloudpi:latest
    ports:
    - published: 80
      target: 80
    - published: 443
      target: 443
    - published: 4443
      target: 4443
    restart: unless-stopped
    volumes:
    - ncdata:/data:rw
    - /etc/localtime:/etc/localtime:ro
version: '3.3'
volumes:
  ncdata:
    external: false

```

<!--
[Notes - Installation Commands][cmd-install]
[cmd-install]: https://gist.github.com/ZendaiOwl/9d4184aac07e2f888201d227a0fa2b39
-->

#### Docker Run 

A working `docker run` command with the `--init` flag for PID 1 management and reaping of zombie processes.

```bash
docker run --init \
-p 4443:4443 \
-p 443:443 \
-p 80:80 \
--volume ncdata:/data \
--name nextcloudpi \
--detach ownyourbits/nextcloudpi:latest \
"$(ip addr | grep 192 | awk '{print $2}' | cut -b 1-14)"
```

- `"$(ip addr | grep 192 | awk '{print $2}' | cut -b 1-14)"`

_Greps an IP-address beginning with 192, modify to fit your system, test in terminal._

#### Dockerfile

[Docker docs, Dockerfile reference][dockerfile-ref]

[dockerfile-ref]: https://docs.docker.com/engine/reference/builder/

Naming scheme

- `Dockerfile.name`

Use `ADD` in Dockerfile to import scripts

- `ADD ${URL} ${PATH}` 

URL to fetch scripts in raw text

- `https://raw.githubusercontent.com/${OWNER}/${REPO}/${BRANCH}/${PATH}`

```docker
Image: bash
Image: scratch (?)

ARG OWNER ["nextcloud"]
ARG REPO ["nextcloudpi"]
ARG BRANCH ["master"]
ARG PATH ["bin/ncp"]
ARG CATEGORY ["BACKUPS"]
ARG SCRIPT ["nc-backup-auto.sh"]
ARG URL ["https://raw.githubusercontent.com"]
ARG PATH_BASH ["/usr/local/bin/bash"]

ADD ["${URL}/${OWNER}/${REPO}/${BRANCH}/${PATH}/${CATEGORY}/${SCRIPT}", "${PATH}/${CATEGORY}/${SCRIPT}"]
COPY --from=bash ["$PATH_BASH", "$PATH_BASH"]
RUN ["$PATH_BASH","-c","chmod","+x","${PATH}/${CATEGORY}/${SCRIPT}"]
SHELL ["$PATH_BASH"]
ENTRYPOINT ["$PATH_BASH","-c","${PATH}/${CATEGORY}/${SCRIPT}"]
```

#### Dockerized Bash Scripts - Examples

1. [Transforming Bash Script to Docker Compose][ex1]  
2. [Automatic Docker Container creation w/bash script][ex2]  
3. [Docker w/Shell script or Makefile][ex3]  
4. [Run scripts, Docker arguments][ex4]  
5. [Run a scripts inside Docker container using Shell script][ex5]  
6. [Run Script, with dev docker image][ex6]  

[ex1]: https://fiberplane.dev/blog/transforming-bash-scripts-into-docker-compose/
[ex2]: https://assistanz.com/automatic-docker-container-creation-via-linux-bash-script/
[ex3]: https://ypereirareis.github.io/blog/2015/05/04/docker-with-shell-script-or-makefile/
[ex4]: https://devopscube.com/run-scripts-docker-arguments/
[ex5]: https://www.commands.dev/workflows/run_a_script_inside_a_docker_container_using_a_shell_script
[ex6]: https://gist.github.com/austinhyde/2e39c01d6b0ebf4aef7409e129c47ea7

#### [CATEGORIES][dirCategories]

* Conversion progress, check the box when the category has been completely converted to individual container images

- [ ] 1. [BACKUPS][dirBackups]
- [ ] 2. [CONFIG][dirConfig]
- [ ] 3. [NETWORKING][dirNetworking]
- [ ] 4. [SECURITY][dirSecurity]
- [ ] 5. [SYSTEM][dirSystem]
- [ ] 6. [TOOLS][dirTools]
- [ ] 7. [UPDATES][dirUpdates]

<!-- START Directory links -->
[dirCategories]: https://github.com/nextcloud/nextcloudpi/tree/containerize/bin/ncp
[dirBackups]: https://github.com/nextcloud/nextcloudpi/tree/containerize/bin/ncp/BACKUPS
[dirConfig]: https://github.com/nextcloud/nextcloudpi/tree/containerize/bin/ncp/CONFIG
[dirNetworking]: https://github.com/nextcloud/nextcloudpi/tree/containerize/bin/ncp/NETWORKING
[dirSecurity]: https://github.com/nextcloud/nextcloudpi/tree/containerize/bin/ncp/SECURITY
[dirSystem]: https://github.com/nextcloud/nextcloudpi/tree/containerize/bin/ncp/SYSTEM
[dirTools]: https://github.com/nextcloud/nextcloudpi/tree/containerize/bin/ncp/TOOLS
[dirUpdates]: https://github.com/nextcloud/nextcloudpi/tree/containerize/bin/ncp/UPDATES
<!-- END Directory links -->

---

#### Scripts, Dependencies & Packages

> _IMPORTANT_

Script shebang must be `#!/usr/bin/env bash` and not `#!/bin/bash`, to be compatible with the `bash` [docker image][bash] natively. 

> Notes  
> There are a few main things that are important to note regarding this image:
> 
> Bash itself is installed at /usr/local/bin/bash, not /bin/bash, so the recommended shebang is #!/usr/bin/env bash, not #!/bin/bash (or explicitly running your script via bash /.../script.sh instead of letting the shebang invoke Bash automatically). The image does not include /bin/bash, but if it is installed via the package manager included in the image, that package will install to /bin/bash and might cause confusion (although /usr/local/bin is ahead of /bin in $PATH, so as long as plain bash or /usr/bin/env are used consistently, the image-provided Bash will be preferred).
> 
> Bash is the only thing included, so if your scripts rely on external tools (such as jq, for example), those will need to be added manually (via apk add --no-cache jq, for example).

|File|Repository|Installed|Dependency
:-:|:-:|:-:|:-:
`library.sh`|`/etc/library.sh`|`/usr/local/etc/library.sh`|`$ncc`,`$ARCH`,`$NCPCFG`,`$CFGDIR`,`$BINDIR`,`NCDIR`
`ncc`|`/bin/ncc`|`/usr/local/bin/ncc`|`occ`,`$NCDIR`
`ncp.cfg`|`/etc/ncp.cfg`|`/usr/local/etc/ncp.cfg`|`-`

|VARIABLES|ENVIRONMENT|
-:|:-
`$ncc`|`/usr/local/bin/ncc`
`$CFGDIR`|`/usr/local/etc/ncp-config.d/`
`$BINDIR`|`/usr/local/bin/ncp/`
`$NCDIR`|`/var/www/nextcloud/`
`$NCPCFG`|`"${NCPCFG:-etc/ncp.cfg}"`
`$ARCH`|`"$(dpkg --print-architecture)"`
`$DESTDIR`|``
`$INCLUDEDATA`|``
`$COMPRESS`|``
`$BACKUPLIMIT`|``
`$BACKUPDAYS`|``
`$NCLATESTVER`|`$(jq -r .nextcloud_version < "$NCPCFG")`
`$PHPVER`|`$(jq -r .php_version < "$NCPCFG")`
`$RELEASE`|`$(jq -r .release < "$NCPCFG")`
`$NEXTCLOUD_URL`|`https://localhost sudo -E -u www-data "/var/www/nextcloud/apps/notify_push/bin/${ARCH}/notify_push" --allow-self-signed /var/www/nextcloud/config/config.php &>/dev/null &`

PACKAGES|
:-:
`dpkg`|
`bash`|
`jq`|
`apt`|
`dialog`|
`cat`|
`awk`|
`mktemp`|
`sudo`|

USER|
:-:
`www-data`|

#### [BACKUPS][dirBackups]
 
- [ ] 1. [nc-backup-auto.sh][nc-backup-auto.sh]
  - Dependencies & Packages
    - library.sh `/usr/local/etc/library.sh`
    - ncp-backup `/usr/local/bin/ncp-backup`
    - metrics.sh `SYSTEM/metrics.sh`
- [ ] 2. [nc-backup.sh][nc-backup.sh]
- [ ] 3. [nc-export-ncp.sh][nc-export-ncp.sh]
- [ ] 4. [nc-import-ncp.sh][nc-import-ncp.sh]
- [ ] 5. [nc-restore-snapshot.sh][nc-restore-snapshot.sh]
- [ ] 6. [nc-restore.sh][nc-restore.sh]
- [ ] 7. [nc-rsync-auto.sh][nc-rsync-auto.sh]
- [ ] 8. [nc-rsync.sh][nc-rsync.sh]
- [ ] 9. [nc-snapshot-auto.sh][nc-snapshot-auto.sh]
- [ ] 10. [nc-snapshot-sync.sh][nc-snapshot-sync.sh]
- [ ] 11. [nc-snapshot.sh][nc-snapshot.sh]

<!-- START BACKUPS Script links -->
[nc-backup-auto.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-backup-auto.sh
[nc-backup.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-backup.sh
[nc-export-ncp.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-export-ncp.sh
[nc-import-ncp.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-import-ncp.sh
[nc-restore-snapshot.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-restore-snapshot.sh
[nc-restore.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-restore.sh
[nc-rsync-auto.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-rsync-auto.sh
[nc-rsync.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-rsync.sh
[nc-snapshot-auto.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-snapshot-auto.sh
[nc-snapshot-sync.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-snapshot-sync.sh
[nc-snapshot.sh]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/BACKUPS/nc-snapshot.sh
<!-- END BACKUPS links -->

#### [CONFIG][dirConfig]

- [ ] 1. [nc-admin.sh][nc-admin]
- [ ] 2. [nc-database.sh][nc-database]
- [ ] 3. [nc-datadir.sh][nc-datadir]
- [ ] 4. [nc-httpsonly.sh][nc-httpsonly]
- [ ] 5. [nc-init.sh][nc-init]
- [ ] 6. [nc-limits.sh][nc-limits]
- [ ] 7. [nc-nextcloud.sh][nc-nextcloud]
- [ ] 8. [nc-passwd.sh][nc-passwd]
- [ ] 9. [nc-prettyURL.sh][nc-prettyURL]
- [ ] 10. [nc-previews-auto.sh][nc-previews-auto]
- [ ] 11. [nc-scan-auto.sh][nc-scan-auto]
- [ ] 12. [nc-trusted-domains.sh][nc-trusted-domains]
- [ ] 13. [nc-webui.sh][nc-webui]

<!-- START CONFIG Script links -->
[nc-admin]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-admin.sh
[nc-database]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-database.sh
[nc-datadir]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-datadir.sh
[nc-httpsonly]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-httpsonly.sh
[nc-init]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-init.sh
[nc-limits]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-limits.sh
[nc-nextcloud]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-nextcloud.sh
[nc-passwd]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-passwd.sh
[nc-prettyURL]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-prettyURL.sh
[nc-previews-auto]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-previews-auto.sh
[nc-scan-auto]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-scan-auto.sh
[nc-trusted-domains]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-trusted-domains.sh
[nc-webui]: https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/CONFIG/nc-webui.sh
<!-- END CONFIG Script links -->

#### [NETWORKING][dirNetworking]

- [ ] 1. [NFS.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/NFS.sh)
- [ ] 2. [SSH.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/SSH.sh)
- [ ] 3. [dnsmasq.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/dnsmasq.sh)
- [ ] 4. [duckDNS.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/duckDNS.sh)
- [ ] 5. [freeDNS.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/freeDNS.sh)
- [ ] 6. [letsencrypt.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/letsencrypt.sh)
- [ ] 7. [namecheapDNS.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/namecheapDNS.sh)
- [ ] 8. [nc-forward-ports.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/nc-forward-ports.sh)
- [ ] 9. [nc-static-IP.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/nc-static-IP.sh)
- [ ] 10. [nc-trusted-proxies.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/nc-trusted-proxies.sh)
- [ ] 11. [no-ip.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/no-ip.sh)
- [ ] 12. [samba.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/samba.sh)
- [ ] 13. [spDYN.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/NETWORKING/spDYN.sh)

#### [SECURITY][dirSecurity]

- [ ] 1. [UFW.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SECURITY/UFW.sh)
- [ ] 2. [fail2ban.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SECURITY/fail2ban.sh)
- [ ] 3. [modsecurity.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SECURITY/modsecurity.sh)
- [ ] 4. [nc-audit.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SECURITY/nc-audit.sh)
- [ ] 5. [nc-encrypt.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SECURITY/nc-encrypt.sh)

#### [SYSTEM][dirSystem]

- [ ] 1. [metrics.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SYSTEM/metrics.sh)
- [ ] 2. [nc-automount.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SYSTEM/nc-automount.sh)
- [ ] 3. [nc-hdd-monitor.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SYSTEM/nc-hdd-monitor.sh)
- [ ] 4. [nc-hdd-test.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SYSTEM/nc-hdd-test.sh)
- [ ] 5. [nc-info.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SYSTEM/nc-info.sh)
- [ ] 6. [nc-ramlogs.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SYSTEM/nc-ramlogs.sh)
- [ ] 7. [nc-swapfile.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SYSTEM/nc-swapfile.sh)
- [ ] 8. [nc-zram.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/SYSTEM/nc-zram.sh)

#### [TOOLS][dirTools]

- [ ] 1. [nc-fix-permissions.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/TOOLS/nc-fix-permissions.sh)
- [ ] 2. [nc-format-USB.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/TOOLS/nc-format-USB.sh)
- [ ] 3. [nc-maintenance.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/TOOLS/nc-maintenance.sh)
- [ ] 4. [nc-previews.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/TOOLS/nc-previews.sh)
- [ ] 5. [nc-scan.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/TOOLS/nc-scan.sh)

#### [UPDATES][dirUpdates]

- [ ] 1. [nc-autoupdate-nc.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/UPDATES/nc-autoupdate-nc.sh)
- [ ] 2. [nc-autoupdate-ncp.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/UPDATES/nc-autoupdate-ncp.sh)
- [ ] 3. [nc-notify-updates.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/UPDATES/nc-notify-updates.sh)
- [ ] 4. [nc-update-nc-apps-auto.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/UPDATES/nc-update-nc-apps-auto.sh)
- [ ] 5. [nc-update-nc-apps.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/UPDATES/nc-update-nc-apps.sh)
- [ ] 6. [nc-update-nextcloud.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/UPDATES/nc-update-nextcloud.sh)
- [ ] 7. [nc-update.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/UPDATES/nc-update.sh)
- [ ] 8. [unattended-upgrades.sh](https://github.com/nextcloud/nextcloudpi/blob/containerize/bin/ncp/UPDATES/unattended-upgrades.sh)

---

### Existing Containers

- [Nextcloud][nextcloud]
- [Linuxserver.io/Nextcloud][linuxserver-nextcloud]
- [MariaDB][mariadb]
- [MySQL][mysql]
- [PHP][php]
- [Debian][debian]
- [Bash][bash]
- [Curl][curl]

<!-- START Container Links -->
[nextcloud]: https://hub.docker.com/_/nextcloud
[linuxserver-nextcloud]: https://hub.docker.com/r/linuxserver/nextcloud  
[mariadb]: https://hub.docker.com/_/mariadb
[mysql]: https://hub.docker.com/_/mysql
[php]: https://hub.docker.com/_/php
[debian]: https://hub.docker.com/_/debian
[bash]: https://hub.docker.com/_/bash
[curl]: https://hub.docker.com/r/curlimages/curl
<!-- END Container Links -->

---