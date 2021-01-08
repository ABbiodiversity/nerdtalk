# 2021-01-08 HTTP basics and shell scripts

## HTTP basics

TLD, URL, parameters, head, body, request/response, protocols:

- https://developer.mozilla.org/en-US/docs/Web/HTTP
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Session
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers
- https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

check some resources (pages, etc) and show around in developer tools

httr vignette

## chow/chmod

https://www.baeldung.com/linux/chown-chmod-permissions

## Shell scripts

Shell novice (loops and scripts): https://swcarpentry.github.io/shell-novice/

### Example 1

- https://github.com/psolymos/qpad-book/blob/master/_build.sh
- https://github.com/psolymos/qpad-book/blob/master/_deploy.sh

### Example 2

`setup.sh`: this is run locally

```bash
bash setup.sh -i ~/.ssh/id_rsa -s root@ip_address -f application.yml
```

The following command line arguments need to be passed to the `setup.sh` script:

- `-i`: your ssh key,
- `-s`: user name (root for DigitalOcean droplets) and the IP address: `user@ip_address`,
- `-f`: path and file name to the yml with the ShinyProxy config, e.g. `/path/to/application-new.yml`.

What the script does:

1. Copies the `application.yml` to the droplet,
2. logs into private registry (optional),
3. pulls the docker images listed in the `application.yml` file,
4. and restarts ShinyProxy.


```bash
#!/bin/bash

# Usage 
#
# `bash setup.sh -i ~/.ssh/id_rsa -s root@ip_address -f application.yml`
#
# -i: ssh key
# -s: user@ip_address
# -f: /path/to/application.yml file

# Registry login
#
# Uncomment lines as needed for registry login.
# Log in to droplet via ssh and add access token into a file:
# `echo your_token > ./token.txt`
# this will be used to pass token via stdin. 
# Change `--username username` to your username.


while getopts i:s:f: flag
do
    case "${flag}" in
        i) key=${OPTARG};;
        s) server=${OPTARG};;
        f) file=${OPTARG};;
    esac
done

echo ">>> Copying $file to droplet"
scp -i $key $file $server:/etc/shinyproxy/application.yml

ssh -i $key $server /bin/bash << EOF
#echo ">>> Logging into registry"
#cat ./token.txt | docker login --username username --password-stdin registry.gitlab.com
echo ">>> Updating docker images according to application.yaml"
wget -O ./update.sh https://raw.githubusercontent.com/analythium/shinyproxy-1-click/master/digitalocean/update.sh
bash ./update.sh /etc/shinyproxy/application.yml
echo ">>> Restarting ShinyProxy"
sudo service shinyproxy restart
echo ">>> Restarting Docker Engine"
sudo service docker restart
rm ./update.sh
echo ">>> Done"
EOF
```

`update.sh`

```bash
#!/bin/bash

# usage:
# `bash update.sh file` where file is the application.yml file

db=$(grep 'container-image:' $1 | sed 's/[^:]*://' | sed 's/^[[:space:]]*//g')
while IFS= read -r line; do docker pull $line; done <<< "$db"
```
