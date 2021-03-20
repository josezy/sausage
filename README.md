### sausage
Server configuration files for Tucano Web Platforms

To use: add `supervisor/sausage.conf` file to supervisor/conf.d programs

## It includes:
* hangar (static homepage)
* ikaro
* recolecta
* cerebro (proxy, needs SSH tunnel)

### SSL certs
##### nginx without docker
```
server {
  listen 80 default_server;
  listen [::]:80 default_server ipv6only=on;
  server_name tucanorobotics.co *.tucanorobotics.co;
  root /var/lib/letsencrypt;

  location ^~ /.well-known/acme-challenge/ {
    default_type "text/plain";
    root /var/lib/letsencrypt;
  }

  location / {
    try_files $uri $uri/ =404;
  }
}
```

`certbot certonly --webroot -w /var/lib/letsencrypt -d tucanorobotics.co -d www.tucanorobotics.co -d icaro.tucanorobotics.co`
* _Remember to add other subdomains_

### SSH tunnel
```
ssh -N -o "ServerAliveInterval 60" -L <source-port>:localhost:<dest-port> -p <remote-port> <remote-user>@<remote-host>
```
