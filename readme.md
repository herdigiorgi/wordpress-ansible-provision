# Easy, secure & fast WordPress install

<img src="imgs/wp.png" alt="wordpress logo" width="250" height="250" align="right">

The objective of this repository is to give you an easy way of installing WordPress. Why? Having a wp server isn't just php + wp files. What about the cache, https, security, plugins, etc?

The features of this repo are:

* Installation of `apache` + `php` 7.2.
* All typically required `php` modules are installed.
* The database is set up in the same server: `mysql` 8.
* Strong and aggressive cache and minification is done via [PageSpeed](https://developers.google.com/speed/) and pre-installed plugins.
* HTTPS ready with [Let’s Encrypt certificates](https://letsencrypt.org/).
* Security in mind.
    * Firewall.
    * [fail2ban](https://www.fail2ban.org).
    * Strong salting and user/passwords.

The code here is written using [Ansible](https://www.ansible.com/) and [Python 3](https://www.python.org/). And the assumptions are the following:

* You are running in linux machine (ansible doens't work under windows).
* You have **root** access to a **ubuntu** server.
* The server has at least 1GB of ram. It wont last a long time running with 512MB or less.

## Usage - quick guide

To install wordpress just to which you already can connect via `ssh` as `root`:

```
$ ./install [your-server-ip]
```

(TODO) With the A dns records pointing to your server, you can enable https:

```
$ ./install [your-server-ip] https [domain-name]
```

(TODO) Inside your instance logged as `root`, retrieve the user and password to login into your new WordPress installation:

```
$ cat /root/passwords/ADMIN_USER
... admin user output ...
$ cat /root/passwords/ADMIN_PASSWORD
... admin password output ...
```

You have your new WordPress installation up and running!

## Usage - step by step

### 0 - Get your *own* server
Most provably you already have your own server, vps, or whatever. If you don't there are many cheap cloud providers for that:

* [Digital Ocean](https://m.do.co/c/288a30cfeece): Very easy, and with free 100 USD for 60 days.
* [AWS LightSail](https://aws.amazon.com/lightsail/): Easy, but with limited functionality.
* [AWS EC2](https://aws.amazon.com/ec2/): Hard, but with extreme flexibility.

### 1 - Check that you can connect
First you need to be able to connect via `ssh` as `root` without specifying manually the key. Like this:

```shell
ssh-add [your-key-path]
ssh root@[your-server-ip]
```

Is very important that you can connect in this way, because the scripts rely on this to work. Also user/password logging will be disabled for security.

If you can connect to your server as root, you are ready to go. Then continue in the next section.

If you had problems connecting:

* `Could not resolve hostname ...`: Try typing the ip.
* `Permission denied (publickey)`: Add your keys first with `ssh-add`.
* `Permissions ... for '...' are too open`: `chmod` the private key to `0400`.
* Troubleshooting guides: [A](https://www.linode.com/docs/troubleshooting/troubleshooting-ssh/), [B](https://www.linux.com/blog/4-reasons-why-ssh-connection-fails%20), [C](https://tecadmin.net/how-to-enable-ssh-as-root-on-aws-ubuntu-instance/).