vader
=====

#### **V**agrant **A**nsible **D**evelopment **E**nvironment for **R**uby.

Spin up a stack for development that is similar to what might be used in production.

Right now the stack just consists of:

* NGINX
* Haproxy
* App Server
 * Ruby 2.0 (via rbenv & ruby-build)
 * Rails 4.0

### Installation             

#### Prerequisites
* [Vagrant](https://github.com/mitchellh/vagrant)
* [Ansible](https://github.com/ansible/ansible)
* [VMWare Fusion](https://www.vmware.com/products/fusion/overview.html) - Could be altered for VirtualBox
* [VMWare Fusion Plugin for Vagrant](http://www.vagrantup.com/vmware).

#### Works For Me
* OS X 10.8.4
* Vagrant 1.3.1
* Ansible 1.3 (master ad595eadea)

#### Clone vader

Clone the repository into a directory where you want to keep your project files.  I place it in `~/src`.

#### Setup Hosts

Edit `/etc/hosts` on your local development machine:

    192.168.2.10    haproxy.dev
    192.168.2.10    rails-test.dev
    
#### Start the VMs

1. `vagrant up app_ruby_1 --provider=vmware_fusion`
2. `vagrant up app_ruby_2 --provider=vmware_fusion`
3. `vagrant up haproxy --provider=vmware_fusion`
4. `vagrant up web --provider=vmware_fusion`

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

#### App Server

* 192.168.2.100 & 192.168.2.101
* Right now just webrick for proof of concept
* Webrick listens on port 3000
