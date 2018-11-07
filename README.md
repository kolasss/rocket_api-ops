# Rocket api server devops

## Installation on single machine

DO server created ubuntu 18.04

### update/upgrade

https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04


created user deploy

https://www.cyberciti.biz/faq/linux-unix-running-sudo-command-without-a-password/

changed ssh port to 6622

configure ufw 

disallow password, root access

### install nginx

https://www.digitalocean.com/community/tutorials/nginx-ubuntu-18-04-ru

### install ruby(rbenv) 2.5.1

https://gorails.com/deploy/ubuntu/18.04

before install

`sudo apt install libssl-dev libreadline-dev zlib1g-dev build-essential`

### install mongodb 4.0

https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/

```
systemctl start mongod
systemctl enable mongod
```

### install redis-server

https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-redis-on-ubuntu-18-04

```
sudo add-apt-repository ppa:chris-lea/redis-server
sudo apt update
sudo apt install redis-server
```

### generate ssh key

https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/

ssh-keygen -t rsa -b 4096 -C "spacexbureau@gmail.com"

### clone project

```
git clone git@github.com:kolasss/rocket_api.git
gem install bundler
rbenv rehash
```

### configure on server for production

set `export RAILS_ENV=production` in .bashrc

`echo 'gem: --no-document' >> ~/.gemrc`

to `/home/deploy/.bundle/config` (not working?)
```
---
BUNDLE_DEPLOYMENT: "true"
BUNDLE_WITHOUT: "development:test"
```

copy `config/master.key`

### bundle install

`bundle install --without development test --deployment`

### configure puma

https://www.digitalocean.com/community/tutorials/how-to-deploy-a-rails-app-with-puma-and-nginx-on-ubuntu-14-04

https://github.com/puma/puma/blob/master/docs/systemd.md

`sudo nano /etc/systemd/system/rocket.puma.service`

Env PATH in service config need to be set of deploy user env

todo: for real production configure `threads_count`

`mkdir tmp/sockets`

https://github.com/puma/puma/blob/master/examples/config.rb

### confgiure nginx

`rocket_api.nginx.conf`


### configure logrotate

https://www.digitalocean.com/community/tutorials/how-to-manage-logfiles-with-logrotate-on-ubuntu-16-04

`sudo nano /etc/logrotate.d/rocket_api`

### letsencrypt certificate

`sudo certbot --nginx -d back.foodkit.com.ua`

https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04

### configure sidekiq

`sudo nano /etc/systemd/system/rocket.sidekiq.service`

`sudo systemctl enable rocket.sidekiq.service`

# TODO

ansible