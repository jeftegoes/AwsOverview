# AWS Route 53<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What is DNS?](#1-what-is-dns)
  - [1.1. DNS Terminologies](#11-dns-terminologies)

# 1. What is DNS?

- Domain Name System which translates the human friendly hostnames into the machine IP addresses:
  - www.google.com => 172.217.18.36
  - DNS is the backbone of the Internet
  - DNS uses hierarchical naming structure:
    - example.com
    - .com
    - api.example.com
    - www.example.com

## 1.1. DNS Terminologies

- **Domain Registrar:** Amazon Route 53, GoDaddy, ...
- **DNS Records:** A, AAAA, CNAME, NS, ...
- **Zone File:** contains DNS records.
- **Name Server:** resolves DNS queries (Authoritative or Non-Authoritative).
- **Top Level Domain (TLD):** .com, .us, .in, .gov, .org, ...
- **Second Level Domain (SLD):** amazon.com, google.com, ...

![Structure of URL](Images/StructureOfUrl.png)
