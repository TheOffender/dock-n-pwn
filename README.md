# Dock-n-Pwn Lab

## Overview

The Dock-n-Pwn lab is a Dockerized penetration testing environment that provides an attack machine (Parrot Security OS) and several vulnerable targets for practicing offensive security techniques. The lab includes core services such as Juice Shop, WebGoat, DVWA, and Metasploitable2, along with an integrated Vulhub directory containing additional vulnerable containers. This setup is designed to be modular so that students can add or remove Vulhub targets as needed.

## Prerequisites

Before you begin, ensure that your system meets the following requirements:

- **Docker:** Make sure Docker is installed. Refer to [Docker's official installation guide](https://docs.docker.com/get-docker/) for instructions.
- **Docker Compose:** Docker Compose is required to orchestrate multiple containers. Installation instructions can be found [here](https://docs.docker.com/compose/install/).
- A basic understanding of the command line and Docker concepts.

## Repository Structure

```
dock-n-pwn/
├── LICENSE
├── README.md
├── docker-compose.yml
├── parrot-share/      # Shared folder between host and parrot container
└── vulhub/            # Vulhub repository containing additional vulnerable containers
```

## Core Lab Setup

The lab is defined in the `docker-compose.yml` file and includes the following services:

- **parrot:** An attack machine running Parrot Security OS. Its persistent data is stored in a named volume (`parrot-home`), and a shared folder (`./parrot-share`) is mounted to `/root/` inside the container.
- **juice-shop:** A vulnerable web application by OWASP Juice Shop.
- **webgoat:** A vulnerable application for learning about common security issues.
- **dvwa:** Damn Vulnerable Web Application for practicing web attacks.
- **metasploitable2:** A vulnerable machine for testing exploits with Metasploit.

The lab uses a single network (`lab-net`) with a defined subnet (`10.10.0.0/24`).

### Starting the Lab

1. **Clone the Repository**  
   Clone the lab repository and navigate into it:
   ```bash
   git clone https://github.com/TheOffender/dock-n-pwn.git
   cd dock-n-pwn
   ```

2. **Launch the Core Lab**  
   Start the lab using Docker Compose:
   ```bash
   docker compose up -d
   ```
   This command starts all the services in detached mode (i.e., in the background) and creates the `lab-net` network if it does not already exist.

## Docker Maintenance and Cleanup

- **Viewing Running Containers:**  
  To list the running containers, use:
  ```bash
  docker ps
  ```

- **Stopping the Lab:**  
  To stop all the lab containers:
  ```bash
  docker compose down
  ```

- **Cleaning Up Unused Resources:**  
  Over time, unused images, stopped containers, and dangling volumes can accumulate. To clean these up, run:
  ```bash
  docker system prune -a
  ```
  **Note:** This command will remove all stopped containers, unused networks, and dangling images. Use with caution.

## Adding a Vulhub Target

The `vulhub` directory contains multiple vulnerable Docker Compose setups from the [Vulhub repository](https://github.com/vulhub/vulhub). To add a Vulhub target to your lab:

1. **Navigate to the Specific Vulhub Vulnerability Directory:**  
   For example, if you want to work with a target for CVE-2023-46604:
   ```bash
   cd parrot-share/vulhub/activemq/CVE-2023-46604/
   ```

2. **Launch the Vulhub Container:**  
   Start the vulnerable container:
   ```bash
   docker compose up -d
   ```

3. **Connect the Vulhub Container to the Lab Network:**  
   Ensure that the newly started Vulhub container is accessible from your lab by connecting it to the `lab-net` network:
   ```bash
   docker network connect dock-n-pwn_lab-net <container_name>
   ```
   Replace `<container_name>` with the actual name of the Vulhub container (for example, it might be `cve-2023-46604-activemq-1`), get this from `docker ps` or from the docker-compose.yml file in the specific vulhub directory.

## Removing a Vulhub Target

When you no longer need a Vulhub target, follow these steps:

1. **Stop and Remove the Vulhub Container:**  
   In the specific Vulhub directory, run:
   ```bash
   docker compose down
   ```

2. **Disconnect the Container from the Lab Network (if necessary):**  
   If the container remains connected to the network, disconnect it using:
   ```bash
   docker network disconnect dock-n-pwn_lab-net <container_name>
   ```

## Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Vulhub GitHub Repository](https://github.com/vulhub/vulhub)

## Conclusion

This lab environment provides a flexible and modular platform for practicing penetration testing techniques. The core services offer a consistent baseline, while the Vulhub targets can be added or removed as needed for specific exercises. Regular maintenance using Docker's cleanup tools will ensure that your environment remains responsive and clutter-free.


