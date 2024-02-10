# 04 Computer networking

## Task

For week-4 you need to go over our 📝 [Class Notes](https://github.com/allops-solutions/devops-aws-mentorship-program/blob/main/devops-mentorship-program/03-march/week-4-070323/00-class-notes.md#-class-notes) and read them carefully.

Also you need to watch and read materials inside 📝 Class Notes -> 📖 [Reading materials](https://github.com/allops-solutions/devops-aws-mentorship-program/blob/main/devops-mentorship-program/03-march/week-4-070323/00-class-notes.md#-reading-materials)

The outcomes of this task should be the following:

*   You need to know what the OSI model is
*   What protocols are
*   IPv4 addressing (being able to create your own subnet), for example, if you have a CIDR block `10.0.0.0/24` and you are asked to create 2 subnets that belong to that CIDR block, where you will have 128 IP addresses available in each subnet you should be able to do subnetting manually using pen and paper.
*   Difference between private and public IPv4 addresses
*   What is IPv6, why we need IPv6 and what is main difference between IPv4 and IPv6
*   What is a client-server architecture
*   Who is the client and who is server
*   What is the TCP protocol
*   What is HTTP protocol and why we are using it
*   What is the main difference between TCP and UDP protocols
*   What is FQDN
*   What is DNS, why do we need it, how it works
*   What is VPN

If you carefully go over 📝 Class Notes and 📖 Reading materials you should not have an issue answering questions from above.

## Task solution

OSI Model: The OSI (Open Systems Interconnection) model is a conceptual framework that standardizes the functions of a telecommunication or networking system into seven distinct layers. It helps in understanding and organizing network protocols and communication processes.

Protocols: Protocols are a set of rules and conventions that govern how data is transmitted and received in a network. They ensure that devices can communicate effectively.

IPv4 Addressing and Subnetting: IPv4 (Internet Protocol version 4) is a standard for identifying devices on a network using a 32-bit address. Subnetting involves dividing an IP address range into smaller subnetworks. Given a CIDR block like 10.0.0.0/24, you can create two subnets with 128 IP addresses each by changing the subnet mask. For example, 10.0.0.0/25 and 10.0.0.128/25 would create two subnets.

Private and Public IPv4 Addresses: Private IP addresses are used within a private network and are not routable on the public internet. Public IP addresses are globally unique and are used for devices accessible from the internet.

IPv6: IPv6 (Internet Protocol version 6) is the successor to IPv4. It was introduced to address the shortage of IPv4 addresses and offers a vastly larger address space. The main difference is that IPv6 uses 128-bit addresses compared to IPv4's 32-bit addresses.

Client-Server Architecture: Client-server architecture is a network architecture in which client devices request services or resources from server devices. Servers provide services, while clients request and consume those services.

Client and Server: In a client-server model, the client is a device or software application that requests services or resources from a server. The server is a device or software application that provides those services or resources.

TCP Protocol: TCP (Transmission Control Protocol) is a connection-oriented protocol that ensures reliable, error-checked data transfer between devices in a network. It establishes a connection, maintains it, and guarantees that data is delivered in order and without errors.

HTTP Protocol: HTTP (Hypertext Transfer Protocol) is an application layer protocol used for transferring web pages and other resources over the internet. It defines how clients and servers communicate to request and deliver web content.

Difference Between TCP and UDP: The main difference is that TCP is connection-oriented, ensuring reliable data delivery, while UDP (User Datagram Protocol) is connectionless and provides faster but less reliable data transmission.

FQDN (Fully Qualified Domain Name): An FQDN is a domain name that specifies a unique and complete address for a specific location on the internet. It includes the hostname and the top-level domain (TLD), e.g., "www.example.com."

DNS (Domain Name System): DNS is a system that translates human-readable domain names (like example.com) into IP addresses, allowing devices to locate each other on the internet.

VPN (Virtual Private Network): A VPN is a secure network connection established over the internet, providing privacy and security by encrypting data and masking the user's IP address. It is used for remote access, privacy, and security reasons.