vader
=====

#### Vagrant
#### Ansible
#### Development
#### Environment for
#### Ruby.

Spin up a stack for development that is similar to what might be used in production.

Right now the stack consists of:

* NGINX 1.4.7
* Haproxy 1.4.25
* PostgreSQL 9.3
* Ruby 2.1 (via rbenv & ruby-build)
* Rails 4.1

### Tested To Work On
* OS X 10.9.2
* Vagrant 1.5.2
* Ansible 1.5 (master 29110c31ea)

### Installation             

#### Prerequisites
* [Vagrant](https://github.com/mitchellh/vagrant) 
* [Ansible](https://github.com/ansible/ansible)
* [VMWare Fusion](https://www.vmware.com/products/fusion/overview.html) - Could be altered for VirtualBox
* [VMWare Fusion Plugin for Vagrant](http://www.vagrantup.com/vmware).

#### Clone vader

Clone the repository into a directory where you want to keep your project files.  I place it in `~/src`.

#### Setup Hosts

Edit `/etc/hosts` on your local development machine:

    192.168.2.10    haproxy.dev
    192.168.2.10    rails-test.dev
    
#### Start the VMs

1. `vagrant up app_ruby_1 --provider=vmware_fusion`
2. `vagrant up app_ruby_2 --provider=vmware_fusion`
3. `vagrant up postgres --provider=vmware_fusion`
4. `vagrant up haproxy --provider=vmware_fusion`
5. `vagrant up web --provider=vmware_fusion`

You should let both of the app servers finish starting up before starting HAproxy, as it will need to pull information from them.

#### Create Your Rails App

    vagrant ssh app_ruby_1
    $ cd /srv/www/
    $ rails new rails_test
    
#### Start Your Rails App

    vagrant ssh app_ruby_1
    $ cd /srv/www/rails_test
    $ rails s -d
    $ exit

And...    

    vagrant ssh app_ruby_2
    $ cd /srv/www/rails_test
    $ rails s -d
    $ exit

#### Take A Peek

* [HAProxy Stats](http://haproxy.dev/haproxy?admin)
* [Rails Test](http://rails-test.dev)

### The Setup

#### NGINX

* 192.168.2.10
* Listening on port 80
* At the moment, all requests sent on to HAProxy

#### HAProxy

* 192.168.2.20
* Listening on port 80 & 8080
* Port 80 services HAProxy's stats UI
* Port 8080 is balanced out to the 2 app servers.

#### PostgreSQL

* 192.168.2.30
* Listening on port 5432
* Will accept requests from app servers (configured in pg_hba.conf)

#### App Server

* 192.168.2.100 & 192.168.2.101
* Right now just webrick for proof of concept
* Webrick listens on port 3000
