# ubuntu-sshd

Dockerized SSH service, built on top of [official Ubuntu](https://registry.hub.docker.com/_/ubuntu/) images.

## Image tags

- rastasheep/ubuntu-sshd:12.04 (precise)
- rastasheep/ubuntu-sshd:12.10 (quantal)
- rastasheep/ubuntu-sshd:13.04 (raring)
- rastasheep/ubuntu-sshd:13.10 (saucy)
- rastasheep/ubuntu-sshd:14.04 (trusty)
- rastasheep/ubuntu-sshd:16.04 (xenial)
- rastasheep/ubuntu-sshd:18.04 (bionic)

## Installed packages

Base:

- [Precise (12.04) minimal](http://packages.ubuntu.com/precise/ubuntu-minimal)
- [Quantal (12.10) minimal](http://packages.ubuntu.com/quantal/ubuntu-minimal)
- [Raring (13.04) minimal](http://packages.ubuntu.com/raring/ubuntu-minimal)
- [Saucy (13.10) minimal](http://packages.ubuntu.com/saucy/ubuntu-minimal)
- [Trusty (14.04) minimal](http://packages.ubuntu.com/trusty/ubuntu-minimal)
- [Xenial (16.04) minimal](http://packages.ubuntu.com/xenial/ubuntu-minimal)
- [Bionic (18.04) minimal](http://packages.ubuntu.com/bionic/ubuntu-minimal)

Image specific:
- [openssh-server](https://help.ubuntu.com/community/SSH/OpenSSH/Configuring)

Config:

  - `PermitRootLogin yes`
  - `UsePAM no`
  - exposed port 22
  - default command: `/usr/sbin/sshd -D`
  - root password: `root`

## Build example

```bash
$ mkdir -p 24.04
$ ./generate.sh
$ sudo docker build -t chGoodchild/ubuntu-sshd:24.04 -f ./24.04/Dockerfile .
$ ssh -p 2222 root@[VPS_IP_ADDRESS]
$ ./generate.sh 
$ sudo docker run -d -p 2222:22 --name [CONTAINER_NAME] chGoodchild/ubuntu-sshd:24.04
$ docker exec -ti [CONTAINER_NAME] passwd
$ docker cp authorized_keys [CONTAINER_NAME]:/root/.ssh/authorized_keys
$ docker exec [CONTAINER_NAME] chown root:root /root/.ssh/authorized_keys
$ docker exec [CONTAINER_NAME] chmod 600 /root/.ssh/authorized_keys
$ docker exec [CONTAINER_NAME] passwd -d root
$ ssh -v -i ~/.ssh/id_rsa -p 2222 root@[VPS_IP_ADDRESS]
```

## Run example

```bash
$ sudo docker run -d -P --name test_sshd rastasheep/ubuntu-sshd:14.04
$ sudo docker port test_sshd 22
  0.0.0.0:49154

$ ssh root@localhost -p 49154
# The password is `root`
root@test_sshd $
```

## Security

If you are making the container accessible from the internet you'll probably want to secure it bit.
You can do one of the following two things after launching the container:

- Change the root password: `docker exec -ti test_sshd passwd`
- Don't allow passwords at all, use keys instead:

```bash
$ docker exec test_sshd passwd -d root
$ docker cp file_on_host_with_allowed_public_keys test_sshd:/root/.ssh/authorized_keys
$ docker exec test_sshd chown root:root /root/.ssh/authorized_keys
```

## Issues

If you run into any problems with this image, please check (and potentially file new) issues on the [rastasheep/ubuntu-sshd](https://github.com/rastasheep/ubuntu-sshd/issues) repo, which is the source for this image.
