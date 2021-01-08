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

Check some resources (pages, etc) and show around in developer tools, show page source, inspect requests/headers/status codes.

Do this from R: [httr vignette](https://cran.r-project.org/web/packages/httr/vignettes/quickstart.html)

## chow/chmod

https://www.baeldung.com/linux/chown-chmod-permissions

- `chown`: change owner, useful to restrict privileges when running scripts
- `chmod`: change mode, useful to make scripts executable (`chmod +x script.sh`)

## Shell scripts

Shell novice (loops and scripts): https://swcarpentry.github.io/shell-novice/

Look at Loops and Shell scripts.

### Example 1

Build and deploy bookdown site (this used Travis and GitHub pages):

- https://github.com/psolymos/qpad-book/blob/master/_build.sh
- https://github.com/psolymos/qpad-book/blob/master/_deploy.sh

### Example 2

How to use flags and have the script act on an input file (find lines and pull images based on that info):

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
3. pulls the docker images listed in the `application.yml` file,
4. and restarts services.


```bash
#!/bin/bash

# Usage 
#
# `bash setup.sh -i ~/.ssh/id_rsa -s root@ip_address -f application.yml`
#
# -i: ssh key
# -s: user@ip_address
# -f: /path/to/application.yml file

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

`update.sh` that is used inside of the previous script

```bash
#!/bin/bash

# usage:
# `bash update.sh file` where file is the application.yml file

db=$(grep 'container-image:' $1 | sed 's/[^:]*://' | sed 's/^[[:space:]]*//g')
while IFS= read -r line; do docker pull $line; done <<< "$db"
```
