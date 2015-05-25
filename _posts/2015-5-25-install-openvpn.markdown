install openvpn client
-------------------------

## install openvpn client

```sh
sudo apt-get install openvpn
```

see version:

```sh
tdp@ubuntu:~$ openvpn --version
OpenVPN 2.3.2 x86_64-pc-linux-gnu [SSL (OpenSSL)] [LZO] [EPOLL] [PKCS11] [eurephia] [MH] [IPv6] built on Apr 13 2015
Originally developed by James Yonan

```


## run openvpn cli

```sh 
sudo openvpn --config client.ovpn
```

notes: put the client config into a directory, it whill include: ca, ovpn etc.

## ssh remote host

```sh
ssh user@remote_ip
```



reference: https://www.astrill.com/knowledge-base/79/OpenVPN---How-to-configure-OpenVPN-with-OpenVPN-client-on-Linux.html