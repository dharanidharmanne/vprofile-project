# vprofile-project: Automated Multi-VM Web Stack Provisioning

This repository contains the **vprofile-project**, a multi-tier Java web application environment automated using **Vagrant** and **VirtualBox**. The project automates the deployment of a complete infrastructure stack, ensuring consistent environments for development and testing.

## Abstract

The **vprofile-project** demonstrates Infrastructure as Code (IaC) by provisioning five distinct virtual machines to host a distributed application. The stack includes a Load Balancer, Web Server, Application Server, Database, Messaging Queue, and Caching service, all configured to communicate within a private network.

The architecture comprises:

* **Nginx**: Acts as the entry point and Load Balancer.
* **Tomcat**: Serves as the Application Server for the Java-based vProfile app.
* **MariaDB**: Provides relational database storage.
* **Memcached**: Handles distributed memory caching.
* **RabbitMQ**: Manages asynchronous messaging between services.

## Methodology

### Infrastructure Representation

* **Virtual Machines**: Defined in the `Vagrantfile` with specific IP addresses (192.168.56.11 through 192.168.56.16) and resource allocations.
* **OS Platforms**: Uses **CentOS Stream 9** for backend services and **Ubuntu Jammy** for the web tier.
* **Networking**: Configured with a `private_network` and `hostmanager` to allow seamless inter-VM communication using hostnames.

### Automated Provisioning

The environment is bootstrapped using modular Shell scripts:

* **Database (`mysql.sh`)**: Installs MariaDB, initializes the "accounts" database, and configures remote access and firewall rules.
* **Caching (`memcache.sh`)**: Sets up Memcached to listen on all interfaces and opens port 11211.
* **Messaging (`rabbitmq.sh`)**: Provisions RabbitMQ and creates a "test" user with administrative privileges.
* **Application (`tomcat.sh`)**: Installs Java 17 and Maven, clones the application source code, builds the `.war` file, and deploys it to Tomcat.
* **Web Tier (`nginx.sh`)**: Configures Nginx as a reverse proxy to route traffic to the Tomcat application server.

## Challenges

* **Resource Intensive**: Requires significant local system memory (RAM) to run five VMs simultaneously.
* **Connectivity**: Managing internal firewalls (`firewalld` and `iptables`) to ensure services can communicate across specific ports like 3306, 11211, and 5672.
* **Dependency Chain**: The application server must wait for backend services to be fully initialized before successfully connecting.

## Future Work

* **Containerization**: Transitioning the VM-based architecture to a Dockerized environment using Docker Compose.
* **Orchestration**: Deploying the stack to a Kubernetes cluster for better scaling and resilience.
* **Security Hardening**: Replacing clear-text passwords in provisioning scripts with encrypted secrets or environment variables.

## How to Use

1. **Clone the repository**:
`git clone <repository-url>`
2. **Install Dependencies**:
Ensure [Vagrant](https://www.vagrantup.com/) and [VirtualBox](https://www.virtualbox.org/) are installed.
3. **Install Vagrant Plugin**:
`vagrant plugin install vagrant-hostmanager`
4. **Provision the stack**:
Run `vagrant up` from the root directory.
5. **Verify**:
Access the application via the Web01 IP: `http://192.168.56.11`.

## References

* Simonis, H. (2005). Sudoku as a constraint problem.
* Vagrant Documentation: Multi-Machine Environments.
* Apache Tomcat and Nginx Documentation.

## Contributors

* **Dharanidhar Manne**
