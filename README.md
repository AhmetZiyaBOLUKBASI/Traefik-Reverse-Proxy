# Traefik Reverse Proxy

This project was born out of my personal curiosity. It was designed as the _**first step**_ in creating an infrastructure that can be used in _**cloud-native environments**_. While planning the architecture of the project, the following requirements were taken into consideration:

- How to host different containerized applications on the same port on a single server with an SSL certificate.
- All applications should be able to be hosted with a single certificate.

Open source applications used in the project:

- _Traefik_
- _Bookstack_
- _MySQL_
- _Mattermost_
- _PostgreSQL_

Additionally, for the LDAP authentication integration of the applications in the project, the GLAuth application was used externally from a different server, also running as a container.

## Overview

![Project Architecture](https://github.com/AhmetZiyaBOLUKBASI/Traefik-Reverse-Proxy/blob/assests/img/ksnip_20250303-002022.png)


In our scenario, we use the Mattermost application for our team's messaging needs and the Bookstack application for documentation. Requests with the "_/mattermost_" extension arriving at port 443 of our server—accessible via the "_myteam.mycompany.com_" domain—will be directed to Mattermost on our server, while requests with the "_/bookstack_" extension will be routed to the Bookstack application. Additionally, we will use requests with the "_/traefik/_" extension to access the Traefik dashboard and those with the "_/api_" extension to access the Traefik API. Only the Bookstack application will access the MySQL database, and only the Mattermost application will access the PostgreSQL database.

Both the Mattermost and Bookstack applications will connect to an **external LDAP server (GLAuth)** for authentication queries.

### Architecture & Design

The solution is architected around the following core components:

- **Traefik Proxy:** The central reverse proxy component that handles routing, SSL management, and load balancing.
- **Backend Services:** Containerized applications that serve as the endpoints for the incoming traffic.
- **Configuration Automation:** Dynamic configuration leveraging labels (in Docker) or annotations (in Kubernetes) to auto-configure routes.
- **Logging & Monitoring:** Integrated with logging systems and dashboards to provide real-time insights into traffic and system performance.

This project leverages [Traefik](https://traefik.io/) as a dynamic reverse proxy to route incoming traffic to your backend services seamlessly. Designed with cloud scalability and automation in mind, this solution is perfect for modern microservices architectures, containerized deployments (e.g., Docker, Kubernetes), and CI/CD pipelines.

### Key Features

- **Dynamic Routing:** Automatically detects and routes traffic to Docker containers or services as they come online.
- **Automatic SSL/TLS:** Integrates with Let’s Encrypt to generate and renew SSL certificates on-the-fly, ensuring secure connections.
- **Load Balancing:** Distributes incoming requests efficiently across multiple backend instances.
- **Easy Integration:** Designed for simple integration with container orchestration platforms like Docker and Kubernetes.
- **Monitoring & Metrics:** Provides integrated support for monitoring via Traefik’s dashboard and real-time metrics to maintain performance.
- **DevOps Best Practices:** Embraces infrastructure-as-code principles, continuous integration/deployment strategies, and automated scaling.

## Getting Started

### Prerequisites

- **Docker**
- A domain name and proper DNS configuration (if using SSL)
- Optional: Access to Let’s Encrypt for automatic certificate provisioning

### Installation

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/AhmetZiyaBOLUKBASI/Traefik-Reverse-Proxy.git
   cd Traefik-Reverse-Proxy

2. **Configure Your Environment:**

   * Update configuration files such as traefik.yml and docker-compose.yml with your specific domain and environment details.
   * Ensure that ports 80, 443, and 8080 (for the dashboard) are open on your firewall.
   * **_(OPTIONAL):_** If you would use your own SSL certificate, you could change certfiles on _/certs_ directory and edit dynamic-conf.yml
   ```yaml
   tls:
     certificates:
      - certFile: /certs/<your SSL.crt>
        keyFile: /certs/<your SSL.key>

3. **Run the Setup:**

   ```bash
   docker-compose up -d

> For GLAuth example, please check [here](https://github.com/rickardl/ldap-mock)

## Verification ##

### Traefik Dashboard:

Access the dashboard at `https://myteam.mycompany.com/traefik/` to monitor routing, active services, and real-time metrics. For first time authentication:
```yaml
username: oks
password: oks123
```

### Traefik API:

Access the API at `https://myteam.mycompany.com/api/version` to monitor routing, active services, and real-time metrics (through API). For authentication:
```yaml
username: oks
password: oks123
```
### Bookstack:

Access the Bookstack at `https://myteam.mycompany.com/bookstack` to document anything

### Mattermost:

Access the Mattermost at `https://myteam.mycompany.com/mattermost` to messaging with your team

### SSL Validation:

You will see the same SSL certificate on every link.(myteam.mycompany.com or your own SSL certificate.)

> This project use GLAuth as LDAP server. If you also use GLAuth as LDAP server with [example](https://github.com/rickardl/ldap-mock), the user and passwords are:
> | **Username** | **Password** | **Group**  |
> | :----------: | :----------: | :--------: |
> | developer    | password     | developers |
> | customer     | password     | users      |
> | service      | password     | services   |

## Use Cases & Benefits

- **_Microservices Environments:_** Ideal for managing a dynamic ecosystem where services scale up or down frequently.
- **_Cloud Scalability:_** Provides a resilient infrastructure capable of handling high loads and ensuring high availability.

## Further Goals To Expand and Develop The Project

For **cloud**, automations can be prepared using tools such as _Terraform_ to enable the project to run in the cloud.

**On the platform side**, to implement DevSecOps practices, the project can be integrated with tools like _Hashicorp Vault_ or _CyberArk Conjur_, and _Helm packages_ can be prepared to deploy it on platforms such as _Kubernetes_.

## Contributing

Contributions, suggestions, and improvements are highly welcome! Please open issues or submit pull requests while following the project's coding and commit guidelines.

## Contact

For further inquiries or discussions, feel free to connect via [LinkedIn](https://www.linkedin.com/in/ahmet-ziya-bolukbasi/) or open an issue on GitHub.
