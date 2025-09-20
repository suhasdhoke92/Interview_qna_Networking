# Interview_qna_Networking

This repository provides a comprehensive guide to key networking concepts, including DNS, the OSI model, proxies, troubleshooting techniques, IP addresses, and AWS subnet configurations. It aims to help you understand and resolve common networking issues in applications and infrastructure.

## What is DNS and Why is it Important?

DNS (Domain Name System) is like the internet's phonebook. It translates human-readable domain names (e.g., example.com) into IP addresses that computers use to communicate. Remembering IP addresses is difficult, so DNS makes browsing the web easier and more intuitive.

## OSI Model

The OSI (Open Systems Interconnection) model is a conceptual framework that describes how data moves through a network in seven layers:

- **Layer 7: Application Layer** - Handles user interactions, e.g., HTTP/HTTPS requests from a browser to example.com.
- **Layer 6: Presentation Layer** - Manages data formatting and encryption, e.g., SSL/TLS for secure communication.
- **Layer 5: Session Layer** - Establishes, maintains, and terminates sessions, e.g., using cookies, caches, and login credentials.
- **Layer 4: Transport Layer** - Segments data into packets, e.g., using TCP/UDP for reliable transmission.
- **Layer 3: Network Layer** - Adds IP addresses and routes packets across networks.
- **Layer 2: Data Link Layer** - Adds MAC addresses for local network communication.
- **Layer 1: Physical Layer** - Transmits raw bits over physical media, e.g., electrical signals through cables.

## Forward Proxy vs Reverse Proxy

- **Forward Proxy**: Sits in front of clients. It acts as a middleman for outgoing requests, e.g., to access restricted websites. Your request goes through the proxy instead of directly to the server.
- **Reverse Proxy**: Sits in front of servers. It handles incoming requests from clients, e.g., for load balancing, whitelisting, blacklisting, or securing APIs using tools like NGINX. Clients access the proxy, which forwards requests to backend servers.
- **Key Difference**: Forward proxies protect or anonymize clients; reverse proxies protect or optimize servers. There are no major similarities between them.

## Troubleshooting Application Slowness

If users report slowness in an application, follow this approach:

1. **Check Server Load**: Monitor CPU and memory usage with tools like `top`, `htop`, Prometheus, Grafana, or Dynatrace. Deprioritize or kill unnecessary processes if needed.
2. **Network Latency**: Use `ping` or `traceroute` to identify delays. Solutions include deploying a CDN or hosting the application closer to clients.
3. **Bandwidth Issues**: Check with `iftop` or `nload`. Optimize traffic or upgrade bandwidth.
4. **Logs and Connections**: Review error logs for delays from upstream servers. Use `netstat` to check for too many open connections and close them if required.

## Curl Works with IP but Fails with Domain (Specific to One Machine)

This is likely a DNS resolution issue:
- The machine's DNS resolver (e.g., from ISP) may fail to resolve domains like google.com.
- Fix: Edit `/etc/resolv.conf` and change the nameserver to a reliable one like `8.8.8.8` (Google DNS).

## Website Returns 502 HTTP Status Code

A 502 Bad Gateway error occurs when a gateway (e.g., load balancer or reverse proxy) cannot get a valid response from the backend server:
- Possible causes: Backend server is down, delayed, or overloaded; database server not responding.
- Troubleshooting:
  - Check NGINX logs (`/var/log/nginx/error.log`).
  - Directly access the backend server.
  - Review backend server logs for issues.

## Difference Between 127.0.0.1 and 0.0.0.0

- **127.0.0.1 (Loopback Address)**: Refers to the local machine itself. It allows the machine to communicate internally. Use for applications accessible only locally, e.g.:
  ```bash
  python3 -m http.server --bind 127.0.0.1 8080
  ```
- **0.0.0.0**: Binds to all IPv4 addresses on the machine, listening on all interfaces (internal and external). Use for applications accessible from anywhere.

## Difference Between Public and Private Subnet

- **Public Subnet**: Has a route to the internet via an Internet Gateway (IGW). Suitable for frontend applications or load balancers that need public access.
- **Private Subnet**: No direct internet access; more secure. Ideal for backend services like databases.

## Fixing Accidental Private Subnet Creation

If you created a private subnet but intended it to be public:
- The subnet exists; you just need to make it public.
- Go to the subnet's route table in AWS VPC console.
- Add a route entry for `0.0.0.0/0` with the IGW as the target.

## Conclusion

Understanding these networking fundamentals helps in diagnosing and resolving common issues in distributed systems. From DNS resolution to proxy configurations and AWS networking, this guide provides practical insights for developers and DevOps engineers to maintain reliable and performant applications.
