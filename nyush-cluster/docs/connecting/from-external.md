# Connecting from External Networks

This page describes how to connect to the NYUSH HPC from external networks (e.g., another university or from your home).

- NYU Shanghai :microscope: users can use
    - the VPN with "vpn.shanghai.nyu.edu".

!!! faq "Getting Help with VPN and Gateway Nodes"

    Please note that the VPNs and gateway nodes are maintained by the central IT departments of NYU Shanghai.
    Authorative information and documentation is provided by the central IT departments as well.

!!! tip "SSH Key Gotchas"

    You should use separate SSH key pairs for your workstation, laptop, home computer etc.
    As a reminder, you will have to register the SSH keys with your home IT organization ([MDC](submit-key/mdc.md) or [Charite](submit-key/charite.md)).
    When using gateway nodes, please make sure to use SSH key agents and agent forwarding (`ssh` flag "`-A`").


## NYU Shanghai Users
Access to NYUSH HPC from external networks requires a VPN connection with special access permissions.

### General VPN Access
You need to apply for general VPN access if you haven't done so already.
The form can be found in the [VPN in IT](https://wiki.it.shanghai.nyu.edu/Virtual-Private-Network-VPN) and contains further instructions.
[IT Helpdesk](mailto:shanghai.it.help@nyu.edu) can help you with any questions.

Once you have been granted VPN access, start the client and connect to VPN.
You will then be able to connect from your client in the VPN just as you do from your workstation.

```bash
$ ssh NetID@hpclogin.shanghai.nyu.edu
```
