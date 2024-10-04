---
{"dg-publish":true,"permalink":"/wp-dev-ucsc-notes/","hide":true,"tags":["work","tech"],"noteIcon":"3","created":"2024-09-05T10:03:07.009-07:00","updated":"2024-10-04T14:42:59.360-07:00"}
---

[[Dashboard\|Dashboard]] | [[Garden Home\|Garden Home]] 

~~Notes on setting up wp-dev.ucsc for development of Gutenberg Blocks Plugin~~.

# Docker and Linux

## VPN

**Cisco Secure Client** connected to Campus VPN, IPv6: 2607:f5f0:110:11::a1
	- **NOTE:** don't connect to VPN until after initial `docker compose up -d` command
## Ports

If a Linux user has **Apache web server** installed, it runs locally as a service and snags `port 80` when the service starts on boot. If a Linux user then runs a Docker configuration that also installs a web server such as **Nginx** they will get a *port already in use* error. This can be fixed by assigning another port such as `8080` or `8008` in the `docker-compose.yml`.

In `docker-compose.yml`, change:
```YAML
ports:
  - 80:80
```

To:
```YAML
ports:
  - 8008:80 # wp-env uses 8080, using 8008 here won't conflict
```
## Permissions

Docker runs transparently in a virtual machine on MacOS and Windows. The VM is run under whatever user installed docker. Docker containers created on these systems are created under the logged-in user.

On Linux, Docker runs as a daemon. Linux users install Docker as `root` and selectively add users to the `docker` group. Docker containers created in Linux are created as `root` because that is what the daemon runs under. 

Users are identified by their user/group ID. root is `0:0`, which is universal, so it's found both locally and in the container. This is why one can drop into root on Linux to work on both (not a good idea).  The local user's user/group ID on Linux (and MacOS) is `1000:1000`. 
### The Issue

The container does not have a user with ID `1000:1000`. The `www-data` user created in the container is `33:33` so files and directories owned by it will not be available to the local Linux user without `sudo`. 

```Bash
-rw-rw-r--  1 jason    jason      21 Sep  7 10:12 .gitignore
drwxrwxr-x  4 jason    jason    4096 Sep  6 06:27 proxy/
drwxr-xr-x  5 www-data www-data 4096 Sep  8 09:46 public/
-rw-rw-r--  1 jason    jason    3196 Sep  7 10:12 README.md
-rwxrwxr-x  1 jason    jason     484 Sep  7 10:12 setup.sh*
```
**wp-dev.ucsc**  creates`public/`, owned by `www-data`, which means a Linux user must use `sudo` to work with the files within and to run `setup.sh` 

In addition, in **wp-dev.ucsc**, the subsequent Docker install scripts ( `docker compose -f docker-compose-install.yml run theme_composer_install`, etc.) install *everything else* under `root`.

```Shell
-rw-r--r--  1 www-data www-data   28 Jun  5  2014 index.php
drwxr-xr-x  7 www-data www-data 4096 Jun 24 10:16 twentytwentyfour/
drwxr-xr-x  7 www-data www-data 4096 Jun 24 10:16 twentytwentythree/
drwxr-xr-x  7 www-data www-data 4096 Jun 24 10:16 twentytwentytwo/
drwxr-xr-x 15 root     root     4096 Sep 10 07:26 ucsc-2022/
```
**wp-dev.ucsc** subsequent install scripts install under `root`

And after all scripts are run, we are unable to connect to the WP database:

```bash
Error: Error establishing a database connection. This either means that the username and password information in your `wp-config.php` file is incorrect or that contact with the database server at `db` could not be established. This could mean your host’s database server is down.
```

### The Fix

The(A) solution is to completely replace the the container's user/group IDs with the local user's ID; replace `www-data` `33:33` with `local-user` `1000:1000`.  

We make this dynamic using a `.env` file and editing the `Dockerfile` and `docker-compose.yml` file.
#### .env

Add the following to the `.env` file:
```Shell
USER_ID=1000
GROUP_ID=1000
```

#### docker-compose.yml

Edit the `docker-compose.yml` file to replace:
```YAML
wp:
  ...
  build: .
  ...
```

with:
```YAML
wp:
  ...
  build:
     context: .
     dockerfile: Dockerfile
  args:
    USER_ID: ${USER_ID:-0}
    GROUP_ID: ${GROUP_ID:-0}
  ...
```

#### Dockerfile

In the `Dockerfile`,  add the following between `FROM wordpress:6.5.5-php8.1-apache` and `RUN set -x \`.
```Shell
ARG USER_ID
ARG GROUP_ID

RUN if [ ${USER_ID:-0} -ne 0 ] && [ ${GROUP_ID:-0} -ne 0 ]; then \
	userdel -f www-data &&\
	if getent group www-data ; then groupdel www-data; fi &&\
	groupadd -g ${GROUP_ID} www-data &&\
	useradd -l -u ${USER_ID} -g www-data www-data &&\
	install -d -m 0755 -o www-data -g www-data /home/www-data && \
	chown --changes --silent --no-dereference --recursive \
		  --from=33:33 ${USER_ID}:${GROUP_ID} \
		 /home/www-data \
;fi
```

This conditional checks to see if local user's user/group ID is defined. If so, it delete's `www-data` user and group. Then, it adds the local user's user/group ID, creates a $HOME directory for it, and `chown`'s the $HOME directory recursively. Creating a $HOME directory enables a Linux user to execute commands inside the container as a non-root user, if necessary.
### Results

After running `docker compose up -d` with  the above changes, the build on a Linux machine will go from this:
```Bash
-rw-rw-r--  1 jason    jason      21 Sep  7 10:12 .gitignore
drwxrwxr-x  4 jason    jason    4096 Sep  6 06:27 proxy/
drwxr-xr-x  5 www-data www-data 4096 Sep  8 09:46 public/
-rw-rw-r--  1 jason    jason    3196 Sep  7 10:12 README.md
-rwxrwxr-x  1 jason    jason     484 Sep  7 10:12 setup.sh*
```

To this:
```Bash
-rw-rw-r--  1 jason jason   21 Sep  7 10:12 .gitignore
drwxrwxr-x  4 jason jason 4096 Sep  6 06:27 proxy/
drwxr-xr-x  5 jason jason 4096 Sep  8 13:43 public/
-rw-rw-r--  1 jason jason 3196 Sep  7 10:12 README.md
-rwxrwxr-x  1 jason jason  484 Sep  7 10:12 setup.sh*
```

### Why bother?

This configuration is OS agnostic, all systems can use it. In this configuration both `USER_ID` and `GROUP_ID` are optional. If neither are defined, this process is skipped. In the spirit of *making the world a better place*, it can be shared with MacOS and Windows users who don't need it and Linux users who do.

*The information above is based on the blog post [Running Docker Containers as Current Host User](https://jtreminio.com/blog/running-docker-containers-as-current-host-user/)*
