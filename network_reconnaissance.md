### Network Reconnaissance
https://sec.cybbh.io/public/security/latest/lessons/lesson-3-research_sg.html

---
#### Host Discovery
1. Determine if hosts exists on the network using quick port agnostic scans.
##### Ping Sweep
Sends one icmp echo request packet to each host on the ```192.168.1.0/24```
* Linux:
  ```bash
  for i in {1..254} ;do (ping -c 1 192.168.1.$i | grep "bytes from" &) ;done
  ```
* Windows: 
  ```bash
    for /L %i in (1,1,255) do @ping -n 1 -w 200 192.168.1.%i > nul && echo 192.168.1.%i is up.
  ```

##### Port Enumeration
2. Determine what ports on a target machine are available to be communicated with. *These ports indicate which services are potentially listening on a target machine and are not blocked by a network or host securrity appliance.*
```bash
nmap -sS -Pn 8.8.8.8 -p 135-139,22,80,443,21,8080
```
Use **nc** to scan a range and specific ports on a discovered machine:
```bash
nc -z -v -w 1 8.8.8.8 440-443
```

##### Port Interrogation
3. Interact with discovered hosts and ports to determine the best way to leverage each available service.
Use nc to interrogatea web server:
* ```nc -Cv 127.0.0.1 80```
* Type: ```GET / HTTP/1.0``` to get a HTTP Response header from the server.

Use nmap to perform service detection on port 22 of your opstation:
* ```nmap -sV 127.0.0.1 -p 22```
Using nikto to perform a vulnerability scan on your opstation:
* ```nikto -h 127.0.0.1 -p 80```
  * Also shows other information like what HTTP methods are allowed and various CVE vulnerabilites

##### Nmap Scripting Engine (NSE) Scripts

---
##### Outcome:
Understand how to perform and utilize advanced scans utilizing Nmap NSE to enhance and develop a accurate penetration test.

#### Advanced Network Scanning
##### Introduction
The Nmap Scripting Engine (NSE) is one of Nmap's most powerful and flexible features. It allows users to write (and share) simple scripts to automate a wide variety of networking tasks. Those scripts are then executed in parallel with the speed and efficiency you expect from Nmap.

Users can rely on the growing and diverse set of scripts distributed with Nmap, or wirte their own to meet custom needs. The core of the Nmap Scripting Engine is an embeddable Lua interpreter. Lua is lightweight language designed for extensibility.

Scripts are stored in a ```scripts``` subdirectory of the Nmap data directory by default:
```cd /usr/share/nmap/scripts```
For efficiency, scripts are indexed in a database stored in:
```cd ../scripts/script.db```

```nmap -Pn -sT -iL $filename -p 1-10000```