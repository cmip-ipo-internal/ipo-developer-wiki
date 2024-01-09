# Configuring the Ports
There are two parts to configuring the ports used by your virutal machine.:

*  The first involves configuring the firewall and opening access to the ports. 

* [optional] The second requires you to allow access to the afforementioned ports on your cloud service provider. 



## Installing the Firewall 

### What is UFW 
Uncomplicated Firewall (UFW) is a user-friendly interface for managing iptables on Linux systems. It simplifies the configuration of firewall rules, making it accessible even for users with limited networking knowledge.

### Installation instructions

```bash
sudo apt update
sudo apt upgrade
sudo apt install ufw
```






### System Commands to check, enable and add ports to the firewall. 



#### Check status, and enable
Check the service status with standard systemctl command:

```sudo systemctl status ufw```


If UFW is not working, we can enable it with:

```sudo ufw enable```

#### Allowing a new port. 

Syntax to open specific TCP port:

```sudo ufw allow (port)/tcp```

__Example__:

```bash

sudo ufw allow 53/tcp

# Syntax supports also names which refer to specific ports:
sudo ufw allow https

# To allow incoming tcp and udp packets on port 21, enter:
sudo ufw allow 21

# Example for specific IP Address:
sudo ufw allow from 190.34.21.113 to any port

```

#### Checking the configuration of the firewall ports. 
Let’s check the configuration:

```sudo ufw status verbose```
Command displays a provisional table with three columns:

##### Explanation:

* `To` Describes the particular protocol

* `Action` Tells us whether it is allowed or denied

* `From` It says about the source e.g anywhere or one ip address like presented above




### Selecting a port. 
We need to select the correct ports for the purpose and only open those which we require. Our choices should ensure that prevent any potential security vulnerabilitues by strategically assigning and managing ports witih a network, 

| Port | Protocol | Main | Description |
|------|----------|-------------|-------------|
| 80   | HTTP     | Yes         | Default port for unsecured web traffic. Widely supported and commonly used. |
| 443  | HTTPS    | Yes         | Default port for secure web traffic using SSL/TLS. Encrypted communication for sensitive data. |
| 8080 | HTTP     | Yes         | Common alternative for HTTP. Often used for testing and development. |
| 8443 | HTTPS    | Yes         | Common alternative for HTTPS. Often used for testing and development with SSL/TLS. |
| 8000 | HTTP     | Yes         | Another alternative for HTTP. Can be used for various applications and testing. |
| 3000 | HTTP     | Yes         | Commonly used in development environments for web applications. |
| 5000 | HTTP     | Yes         | Often used for web applications, including some frameworks like Flask. |
| 8888 | HTTP     | Yes         | Commonly used in development environments, especially with Jupyter Notebooks. |
| 8081  | HTTP     | No          | Often used as an alternative HTTP port, especially in development environments. |
| 8880  | HTTP     | No          | Another alternative for HTTP, sometimes used in testing and development. |
| 9000  | HTTP     | No          | Commonly used in development environments, especially with PHP applications. |
| 8088  | HTTP     | No          | Used in some applications and development scenarios as an alternative HTTP port. |
| 8090  | HTTP     | No          | Frequently used as an alternative HTTP port for various applications. |
| 8181  | HTTP     | No          | Occasionally used for HTTP, especially in testing and development. |
| 8889  | HTTP     | No          | Another alternative for HTTP, sometimes chosen for specific applications. |
| 9090  | HTTP     | No          | Used in some applications and development environments as an alternative HTTP port. |

