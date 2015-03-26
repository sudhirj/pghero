# PgHero Standalone

Packaged for:

- Ubuntu 14.04 (Trusty)
- Ubuntu 12.04 (Precise)
- Debian 7 (Wheezy)
- CentOS and RHEL 6
- Fedora 20
- SUSE Linux Enterprise Server

64-bit only

## Installation

Ubuntu 14.04 (Trusty)

```sh
wget -qO - https://deb.packager.io/key | sudo apt-key add -
echo "deb https://deb.packager.io/gh/pghero/pghero trusty master" | sudo tee /etc/apt/sources.list.d/pghero.list
sudo apt-get update
sudo apt-get install pghero
```

Ubuntu 12.04 (Precise)

```sh
wget -qO - https://deb.packager.io/key | sudo apt-key add -
echo "deb https://deb.packager.io/gh/pghero/pghero precise master" | sudo tee /etc/apt/sources.list.d/pghero.list
sudo apt-get update
sudo apt-get install pghero
```

Debian 7 (Wheezy)

```sh
wget -qO - https://deb.packager.io/key | sudo apt-key add -
echo "deb https://deb.packager.io/gh/pghero/pghero wheezy master" | sudo tee /etc/apt/sources.list.d/pghero.list
sudo apt-get update
sudo apt-get install pghero
```

CentOS and RHEL 6

```sh
sudo rpm --import https://rpm.packager.io/key
echo "[pghero]
name=Repository for pghero/pghero application.
baseurl=https://rpm.packager.io/gh/pghero/pghero/centos6/master
enabled=1" | sudo tee /etc/yum.repos.d/pghero.repo
sudo yum install pghero
```

Fedora 20

```sh
sudo rpm --import https://rpm.packager.io/key
echo "[pghero]
name=Repository for pghero/pghero application.
baseurl=https://rpm.packager.io/gh/pghero/pghero/fedora20/master
enabled=1" | sudo tee /etc/yum.repos.d/pghero.repo
sudo yum install pghero
```

SUSE Linux Enterprise Server

```sh
sudo rpm --import https://rpm.packager.io/key
sudo zypper addrepo "https://rpm.packager.io/gh/pghero/pghero/sles12/master" "pghero"
sudo zypper install pghero
```

## Setup

Add your database.

```sh
sudo pghero config:set DATABASE_URL=postgres://user:password@hostname:5432/dbname
```

And optional authentication.

```ruby
sudo pghero config:set PGHERO_USERNAME=link
sudo pghero config:set PGHERO_PASSWORD=hyrule
```

Start the server - defaults to port `6000`.

```sh
sudo pghero scale web=1
```

Confirm it’s running with:

```ruby
curl -v http://localhost:6000/
```

To open to the outside world, add a proxy. Here’s how to do it with Nginx on Ubuntu.

```sh
sudo apt-get install -y nginx
cat | sudo tee /etc/nginx/sites-available/default <<EOF
server {
  listen          80;
  server_name     "";
  location / {
    proxy_pass    http://localhost:6000;
  }
}
EOF
sudo service nginx restart
```

## Management

Ubuntu, CentOS, and RHEL

```sh
sudo service pghero status
sudo service pghero start
sudo service pghero stop
sudo service pghero restart
```

Debian, Fedora, and SUSE

```sh
#todo
```

## Query Stats

Query stats can be enabled from the dashboard. If you run into issues, [view the guide](Query-Stats.md).

## System Stats

CPU usage is available for Amazon RDS.  Add these variables to your environment:

```sh
sudo pghero config:set PGHERO_ACCESS_KEY_ID=accesskey123
sudo pghero config:set PGHERO_SECRET_ACCESS_KEY=secret123
sudo pghero config:set PGHERO_DB_INSTANCE_IDENTIFIER=epona
```

## Customize

Change the port - you cannot use a privileged port like `80` or `443`

```sh
sudo pghero config:set PORT=6000 # default
```

Minimum time for long running queries

```sh
sudo pghero config:set PGHERO_LONG_RUNNING_QUERY_SEC=60 # default
```

Minimum average time for slow queries

```sh
sudo pghero config:set PGHERO_SLOW_QUERY_MS=20 # default
```

Minimum calls for slow queries

```sh
sudo pghero config:set PGHERO_SLOW_QUERY_CALLS=100 # default
```

Minimum connections for high connections warning

```sh
sudo pghero config:set PGHERO_TOTAL_CONNECTIONS_THRESHOLD=100 # default
```

## Upgrading

Ubuntu and Debian

```sh
sudo apt-get update
sudo apt-get install --only-upgrade pghero
```

CentOS, RHEL, and Fedora

```sh
sudo yum update
sudo yum install pghero
```

SUSE

```sh
sudo zypper update pghero
```

## Credits

:heart: Made possible by [Packager](https://packager.io/)