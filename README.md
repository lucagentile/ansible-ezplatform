# Ansible eZ Platform
Ansible playbooks for setting up a LEMP stack for eZ Platform, based on [https://roots.io/trellis/docs/](https://roots.io/trellis/docs/)

- Local development environment with Vagrant
- High-performance production servers

## What's included

It will configure a server with the following and more:

* Ubuntu 18.04 Bionic LTS
* Nginx (with optional FastCGI micro-caching)
* PHP 7.2
* MariaDB (a drop-in MySQL replacement)
* SSL support (scores an A+ on the [Qualys SSL Labs Test](https://www.ssllabs.com/ssltest/))
* Let's Encrypt for free SSL certificates
* HTTP/2 support (requires SSL)
* Composer
* sSMTP (mail delivery)
* MailHog
* Memcached
* Fail2ban and ferm

## Requirements

Make sure all dependencies have been installed before moving on:

* [Composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx)
* [Virtualbox](https://www.virtualbox.org/wiki/Downloads) >= 4.3.10
* [Vagrant](https://www.vagrantup.com/downloads.html) >= 2.1.0

Then, install Vagrant Landrush and Vagrant Host Manager

```shell
# from ansible directory
$ vagrant plugin install vagrant-hostmanager
$ vagrant plugin install vagrant-landrush
```

**Windows user?**

From the Windows Subsystem for Linux
```shell
$ sudo apt-get install python-pip
$ pip install ansible

# Install a specific Ansible version:
$ pip install ansible==2.4.0.0
```

## Installation

The recommended directory structure for a Trellis project looks like:

```shell
example.com/      # → Root folder for the project
├── ansible/      # → Your clone of this repository
└── site/         # → eZ Platform
```

1. Create a new project directory:
```plain
$ mkdir example.com && cd example.com
```
2. Install Trellis:
```plain
$ git clone --depth=1 git@github.com:roots/trellis.git && rm -rf trellis/.git
```
3. Install eZ Platform into the `site` directory:
```plain
$ composer create-project --keep-vcs ezsystems/ezplatform site 
```

## Configuration
1. Configure your eZ Platform sites in `group_vars/development/ezplatform_sites.yml` 
and in `group_vars/development/vault.yml` accordingly to the already inputed settings during eZ installation
2. Optionally configure the IP address at the top of the `vagrant.default.yml` and in `hosts/development`
to allow for multiple boxes to be run concurrently
3. Ensure you're in the ansible directory: `cd ansible`
4. Run `vagrant up`

## Remote server setup (staging/production)

For remote servers, installing Ansible locally is an additional requirement.

A base Ubuntu 18.04 (Bionic) server is required for setting up remote servers. OS X users must have [passlib](http://pythonhosted.org/passlib/install.html#installation-instructions) installed.

1. Configure your eZ Platform sites in `group_vars/<environment>/ezplatform_sites.yml` and in `group_vars/<environment>/vault.yml`
2. Add your server IP/hostnames to `hosts/<environment>`
3. Specify public SSH keys for `users` in `group_vars/all/users.yml`
4. Run `ansible-playbook server.yml -e env=<environment>` to provision the server

## Deploying to remote servers

1. Add the `repo` (Git URL) of your eZ Platform project in the corresponding `group_vars/<environment>/ezplatform_sites.yml` file
2. Set the `branch` you want to deploy
3. Run `./bin/deploy.sh <environment> <site name>`
4. To rollback a deploy, run `ansible-playbook rollback.yml -e "site=<site name> env=<environment>"`

## TODO
1. Add *legacy* boolean parameter in `group_vars/<environment>/ezplatform_sites.yml`,
and use it for loading ezlegacy rewrite rules in nginx .conf files, and install php 7.2 or 7.3
2. Add *imagick* and *gd* parameter, and use it for installing just one instead of both
3. Use .env variables also for parameters.yml
4. Deployment strategy

## Contributing

Contributions are welcome from everyone.

## License
MIT
