# Vagrant-Debian-Ansible-LEMP  
[![Build Status](https://travis-ci.org/neikei/vagrant-debian-ansible-lemp.svg?branch=master)](https://travis-ci.org/neikei/vagrant-debian-ansible-lemp)

This is a development environment for Symfony projects on a Debian based Vagrantbox from the [bento project](https://github.com/chef/bento) forked from https://github.com/neikei/vagrant-debian-ansible-lemp which also enables one to develeop inside and outside the box; right out of the box, thanks to ansible. 

## Included components

| Software            | Version  | Tested   |
|---------------------|----------|----------|
| Debian              | 8.7      | &#10003; |
| Nginx               | 1.10.3   | &#10003; |
| MySQL               | 5.5.54   | &#10003; |
| Redis               | 3.2.8    | &#10003; |
| PHP                 | 7.1      | &#10003; |
| PHPUnit             | 6.0.9    | &#10003; |
| Composer            | 1.4.1    | &#10003; |
| Varnish             | 4.0      | &#10003; |
| Node.js             | 6.10.0   | &#10003; |
| Symfony             | 3.2.4    | &#10003; |
| Cinnamon      (new) | 2.2.16   | &#10003; |
| GoogleChrome  (new) | latest   | &#10003; |
| SublimeText 3 (new) | latest   | &#10003; |
| PHP-Storm     (new) | 2017.1   | &#10003; |

## Requirements
 - Hypervisor
   - Virtualbox >= 5.1.14
   - Parallels >= 10
 - Vagrant >= 1.9.2
 - Vagrant Plugins:
   - vagrant-hostmanager # necessary for host entries
   - vagrant-vbguest # recommended for virtualbox users
   - vagrant-winnfsd # only for Windows

## Getting started
1. git clone https://github.com/neikei/vagrant-debian-ansible-lemp.git
2. cd vagrant-debian-ansible-lemp
3. vagrant up
4. ... wait ...
5. Check the initial webpage: http://lemp.test/

## Default access

 - Webserver: http://lemp.ldev/
 - Symfony projects: http://example.lemp.ldev
 - Default web root: /vagrant/web
 - Symfony web root: /vagrant/example
 - MySQL: 192.168.56.111:3306
   - user: admin
   - password: changeme
   - root is allowed to access the database from localhost without a password
 - Redis: 127.0.0.1:6379
 - Varnish: 127.0.0.1:6082

## Configuration

Check the config.yml if you want to modify the following settings.

```
configs:
    private_ip: "192.168.56.111"    # VM IP in your host-only-network
    vmname: "symfony-development"   # VM name for Virtualbox or Parallels
    servername: "lemp.test"         # Servername and domain for your projects
    projectnames: ["example"]       # Comma-separated list with your projectnames
```
Every configuration change needs a re-build of the VM. All manual configuration changes on your VM will be lost, but the Symfony projects are safe.

```
vagrant destroy && vagrant up
```

## Feedback, Issues and Pull-Requests

Feel free to report issues, fork this project and submit pull requests.

## Changelog
21 March 2017
 - Improved multi-project handling
 - Added Redis
 - Added Varnish

16 March 2017
 - Node.js installation improvements
 - Updated Nginx to version 1.10.3 from dotdeb

14 March 2017
 - Added hostmanger for local hostfile management
 - Improved PHP, Nginx and Nodejs installation
 - Added Symfony 3.2 compatibility

27 February 2017
 - Updated Debian to version 8.7

22 February 2017
 - Added PHPUnit
 - Added tests for the Vagrantbox
 - Added Travis CI builds

21 February 2017
 - Added nodejs

17 February 2017
 - Improved the PHP provisioning

16 February 2017
 - Changed PHP repository from dotdeb.org to sury.org
 - Updated PHP to version 7.1

14 February 2017
 - Initial Commit
 - Debian 8.6, Nginx, PHP 7.0, MySQL
