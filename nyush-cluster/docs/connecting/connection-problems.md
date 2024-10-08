# Debugging Connection Problems

When you encounter problems with the login to the cluster although we indicated
that you should have access, depending on the issue, here is a list of how to
solve the problem:

### I'm getting a "connection refused"

The full error message looks as follows:

```
ssh: connect to host hpclogin.shanghai.nyu.edu port 22: Connection refused
```

This means that your computer could not open a network connection to the server.

- NYUSH Cluster can be connected to from:
    - Campus network
    - NYUSH VPN :point_right: **but only with [NYUSH VPN](./from-external.md#zusatzantrag-b-recommended)**. :point_left:
- If you think that there is no problem with any of this then please include the output of the following command in your ticket (use the server that you want to read instead of `<DEST>`):
    - Linux/Mac
        ```
        ifconfig
        traceroute <DEST>
        ```
    - Windows
        ```
        ipconfig
        tracepath <DEST>
        ```

## I can connect, but it seems that my account has no access yet

```
You're logging into NYUSH HPC cluster! (hpclogin.shanghai.nyu.edu)

 ***Your account has not been granted cluster access yet.***

 If you think that you should have access, please contact
 shanghai.it.help@nyu.edu for assistance.

 For applying for cluster access, contact shanghai.it.help@nyu.edu.

user@hpclogin.shanghai.nyu.edu's password:
```

!!! hint

    **This is the most common error**, and the main cause for this is a wrong username. Please take a couple of minutes to read the [What is my username?](connecting.md#what-is-my-username)!

If you encounter this message **although we told you that you have access and you checked the username as mentioned above**,
please write to [shanghai.it.help@nyu.edu](mailto:shanghai.it.help@nyu.edu),
always indicating the message you get and a detailed description of what you
did.

## I'm getting a passPHRASE prompt

```
You're logging into NYUSH HPC cluster! (hpclogin.shanghai.nyu.edu)

 *** It looks like your account has access. ***

 Login is based on **SSH keys only**, if you are getting a password prompt
 then please contact shanghai.it.help@nyu.edu for assistance.

Enter passphrase for key '/gpfsnyu/home/USER/.ssh/id_rsa':
```

Here you have to enter the **passphrase that was used for encrypting your private key**.
Read [SSH Basics](./ssh-basics.md#ssh-keys) for further information of what is going on here.

## I can connect, but I get a passWORD prompt

```
You're logging into NYUSH HPC cluster! (hpclogin.shanghai.nyu.edu)

 *** It looks like your account has access. ***

 Login is based on **SSH keys only**, if you are getting a password prompt
 then please contact shanghai.it.help@nyu.edu for assistance.

user@hpclogin.shanghai.nyu.edu's password:
```

!!! important "This is diffeerent from passPHRASE prompt"

    Please see [I'm getting a passPHRASE prompt](#im-getting-a-passphrase-prompt) for more information.

When you encounter this message during a login attempt, there is an issue with
your SSH key. In this case, please connect with increased verbosity to the
cluster (`ssh -vvv ...`) and mail the output and a detailed description to
[shanghai.it.help@nyu.edu](mailto:shanghai.it.help@nyu.edu).

