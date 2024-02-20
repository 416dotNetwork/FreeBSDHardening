# FreeBSD Hardening
Below, we share several strategies that _**416.network**_ employs to fortify their FreeBSD servers. While this list doesn't encompass all our security measures, these steps lay a solid foundation for enhancing security and resilience, tailored to diverse user needs. We are consistently refining and augmenting these measures to proactively address emerging threats.
 - SSH / SSHD Hardening

## SSH / SSHD Hardening
SSH stands for Secure Shell, which is a cryptographic network protocol used to establish a secure connection between a client and a server. It provides a secure channel over an unsecured network, typically for remote login or command execution. SSH encrypts all data sent between the client and the server, including passwords and commands, thereby ensuring confidentiality and integrity.

The SSH Daemon (SSHD) is the server component of the SSH protocol suite. SSHD runs on a server and listens for incoming SSH connections from clients. When a client wants to connect to the server securely, it communicates with the SSHD process running on the server, which then authenticates the client and establishes a secure connection.

A default installation may present certain vulnerabilities within SSH and SSHD that require addressing. 

All of the recommendations are modified within the `/etc/ssh/sshd_config file`. Don't forget that whenever you want to have your modifications to take effect, you must restart the SSH Daemon by running the command `service sshd restart`.

***Port 22, the default port for SSH, and SSHD are exposed directly to the public-facing internet. The port is also bound to all interfaces, making it accessible from any network connected to the system.***

Whether this poses a concern depends on your comfort level with external users accessing SSH. If you're fine with it, you may opt to maintain the current configuration. However, for our services, external SSH access is restricted. No one outside of network can connect. Nevertheless, users within our internal LAN are granted SSH access to all servers running the SSHD Daemon.

By default, the following line is found within the config file:

    #ListenAddress 0.0.0.0
    
We simply un-comment the line and add the Static IP Number of our server .

    ListenAddress 172.16.10.20

***SSHD listens to interfaces on IPV4 and IPV6 by default***

SSHD is configured to connect to interfaces on both the IPv4 and IPv6 stacks. However, since we exclusively utilize IPv4 internally, we opt to disable any settings associated with IPv6 to streamline our configuration.

By default, the following line is found within the config file:

    #AddressFamily any

We simply un-comment the line and make the following modification:

    AddressFamily inet

