# SSH Basics
## What is SSH?
SSH stands for **S** ecure **Sh** ell. It is a software that allows to establish a user-connection to a remote UNIX/Linux machine over the network and remote-control it from your local work-station.

Let's say you have an HPC cluster with hundreds of machines somewhere in a remote data-center and you want to connect to those machines to issue commands and run jobs. Then you would use SSH.

## Getting Started
### Installation
Simply install your distributions `openssh-client` package. You should be able to find plenty of good tutorials online.
On Windows you can consider using [MobaXterm (recommended)](../connecting/connecting-windows.md#install-ssh-client-for-windows) or [Putty](https://www.putty.org/).

### Connecting
Let's call your local machine the client and the remote machine you want to connect to the server.

You will usually have some kind of connection information, like a hostname, IP address and perhaps a port number. Additionally, you should also have received your user-account information stating your user-name, your password, etc.

Follow the instructions below to establish a remote terminal-session.

----

**If your are on Linux**

Open a terminal and issue the following command while replacing all the `<...>` fields with the actual data:

```
# default port
ssh <username>@<hostname-or-ip-address>

# non-default port
ssh <username>@<hostname-or-ip-address> -p <port-number>
```

**If you are on windows**

Start `putty.exe`, go into the `Session` category and fill out the form, then click the `Connect` button.
Putty also allows to save the connection information in different profiles so you don't have to memorize and retype all fields every time you want to connect.

----

