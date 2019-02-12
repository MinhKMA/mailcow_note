# Install mailcow 

## Prepare Your System

- Minimum System Resources

    + OS   : CentOS 7 
    + CPU  : 1 GHz
    + RAM  : 2 GiB + Swap (better: 4 GiB + Swap)
    + Disk : 5 GiB (without emails)

- Firewall & Ports

Please check if any of mailcow's standard ports are open and not in use by other applications:

`netstat -tulpn | grep -E -w '25|80|110|143|443|465|587|993|995'`

With CentOS7, stop and disable service postfix. 

## DNS setup

- The minimal DNS configuration

    ```
    # Name              Type       Value
    mail                IN A       1.2.3.4
    autodiscover        IN CNAME   mail
    autoconfig          IN CNAME   mail

    @                   IN MX 10   mail
    ```

## Install

### Learn how to install Docker and Docker Compose.

- Docker

    ```
    curl -sSL https://get.docker.com/ | sudo sh
    usermod -aG docker `whoami`
    systemctl start docker.service
    systemctl enable docker.service
    systemctl status docker.service
    ```

- Docker-Compose

    ```￼
    curl -L https://github.com/docker/compose/releases/download/$(curl -Ls https://www.servercow.de/docker-compose/latest.php)/docker-compose-$(uname -s)-$(uname -m) > /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    ```

### Clone the master branch of the repository, make sure your umask equals 0022.

```
# umask
0022
# cd /opt
# git clone https://github.com/mailcow/mailcow-dockerized
# cd mailcow-dockerized
```

### Generate a configuration file. Use a FQDN (host.domain.tld) as hostname when asked.

```
./generate_config.sh
Press enter to confirm the detected value '[value]' where applicable or enter a custom value.
Hostname (FQDN): mail.supportdao.com
Timezone [Asia/Ho_Chi_Minh]: Asia/Ho_Chi_Minh
```

### Pull the images and run the composer file. The parameter -d will start mailcow: dockerized detached:

```
docker-compose pull
docker-compose up -d
```

### Redirect HTTP to HTTPS

Open data/conf/nginx/site.conf and add two new server configs at the top of that file:

```
server {
  listen 80;
  listen [::]:80;
  server_name autoconfig.*;
  root /web;
  location / {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass phpfpm:9002;
    include /etc/nginx/fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root/autoconfig.php;
    try_files /autoconfig.php =404;
  }
}
server {
  listen 80 default_server;
  listen [::]:80 default_server;
  include /etc/nginx/conf.d/server_name.active;
  if ( $request_uri ~* "%0A|%0D" ) { return 403; }
  return 301 https://$host$uri$is_args$args;
}
```

and 

```docker-compose restart nginx-mailcow```

Done. 

You can now access https://${MAILCOW_HOSTNAME} with the default credentials admin + password moohoo.