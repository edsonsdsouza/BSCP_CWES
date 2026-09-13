# Web Proxy

Today, most modern web and mobile applications work by continuously connecting to back-end servers to send and receive data and then processing this data on the user's device, like their web browsers or mobile phones. With most applications heavily relying on back-end servers to process data, testing and securing the back-end servers is quickly becoming more important.

To capture the requests and traffic passing between applications and back-end servers and manipulate these types of requests for testing purposes, we need to use Web Proxies.

## What are web proxies?

Web proxies are specialised tools set up between the web/mobile applications and backend servers. They are specialised tools through which we can act as the MITM attacker. Using web proxy we can analyse the request sent by the web/mobile application and response sent by the Backend servers. Furthermore, we can intercept a specific request to modify its data and see how the back-end server handles them, which is an essential part of any web penetration test. While other Network Sniffing applications, like Wireshark, operate by analyzing all local traffic to see what is passing through a network, Web Proxies mainly work with web ports such as, but not limited to, HTTP/80 and HTTPS/443.

While the primary use is to capture and replay the HTTP request, there are several other features the web proxies provide.

1. Web Application Vulnerability Scanning
2. Web Fuzzing
3. Web Crawling
4. Web Application Mapping
5. Web Request Analysis
6. Web Configuration Testing
7. Code Reviews

Web proxy tools: `Burp Suite` and `ZAP`

## Burp Suite

Burp Suite (pronounced Burp Sweet) provides both free and paid features. Paid features include:

- Active Web App Scanner
- Fast Burp Intruder
- The ability to load certain Burp Extensions

## OWASP Zed Attack Proxy (ZAP)

Open Source and no paid feature

