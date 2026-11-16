# Room-404
TryHAckME

# TryHackMe Walkthrough: Room 404 – Hacker Holidays Day 2

## 1. Target Information

| Item | Details |
|---|---|
| Platform | TryHackMe |
| Room | Room 404 |
| Category | Web Security |
| Difficulty | Very Easy |
| Target | Byte Lotus Hotel Web Application |
| Port | 8080 |
| Vulnerability | Exposed Git Repository |
| Main Tools | Gobuster, Git-Dumper, Browser, Linux Terminal |

**Room URL:** [TryHackMe - Room 404](https://tryhackme.com/room/hh-room404-804573bf)

---

## 2. Objective

The objective of this challenge is to identify an exposed Git repository on a web server, retrieve the website's source code and discover a hidden flag.

---

## 3. Methodology Used

The following methodology was used:

1. **Reconnaissance** – Access the target web application and identify the available service.
2. **Directory Enumeration** – Use Gobuster to discover hidden directories and files.
3. **Vulnerability Identification** – Identify the publicly accessible `.git` directory.
4. **Source Code Extraction** – Use Git-Dumper to retrieve the exposed Git repository.
5. **Source Code Analysis** – Examine the downloaded files to identify sensitive information.
6. **Flag Discovery** – Read the README.md file and retrieve the flag.

---

## 4. Step-by-Step Walkthrough

### Step 1: Start the Target Machine

First, access the TryHackMe Room 404 page and start the Lab Machine.

After the machine starts, obtain the assigned IP address.

The target web application runs on port 8080.

**Target URL:**

```bash
http://MACHINE_IP:8080
```

### Step 2: Access the Target Website

Open a browser and enter the target URL.

The Byte Lotus Hotel website will appear.

The website does not directly reveal the flag. Therefore, further investigation is required to identify hidden resources.



### Step 3: Directory Enumeration Using Gobuster

Use Gobuster to identify hidden directories and files on the web server.

**Command:**

```bash
gobuster dir -u http://MACHINE_IP:8080/ \
-w /usr/share/wordlists/dirb/common.txt \
-x html,js,php,txt,json
```

The scan reveals an exposed Git repository:

```text
/.git/HEAD
```

HTTP status code: `200 OK`.

This indicates that the Git repository is publicly accessible.



### Step 4: Verify the Exposed Git Repository

Open the following URL in the browser:

```text
http://MACHINE_IP:8080/.git/
```

The server exposes Git repository information, including files and directories such as:

- HEAD
- config
- objects
- refs
- logs
- index

This confirms an information disclosure vulnerability because internal repository information is accessible without authentication.



### Step 5: Extract the Git Repository

Use Git-Dumper to download and reconstruct the exposed repository.

**Install Git-Dumper:**

```bash
pip install git-dumper
```

**Download the repository:**

```bash
git-dumper http://MACHINE_IP:8080/.git/ ./byte-lotus-source
```

Navigate to the downloaded directory:

```bash
cd byte-lotus-source
```

List the recovered files:

```bash
ls -la
```

The recovered files may include:

- README.md
- app.js
- index.html



### Step 6: Analyse the Source Code

Examine the README.md file because it may contain internal development information.

**Command:**

```bash
cat README.md
```

The file contains internal staging information and a flag that was not supposed to be included in the production environment.



### Step 7: Retrieve the Flag

The README.md file reveals the following flag:

```text
THM{byt3_l0tus_n3v3r_f0rg3ts}
```

This completes the challenge.


---

## 5. Vulnerability Analysis

**Vulnerability Name:** Exposed Git Repository

### Description

An exposed Git repository occurs when a web server allows public access to its `.git` directory. This can happen when developers accidentally deploy the entire development repository into the production web directory.

An attacker may download the repository and recover source code, commit history and sensitive information.

### Security Impact

- Exposure of application source code.
- Disclosure of internal development information.
- Leakage of credentials or configuration data if stored in the repository.
- Increased risk of further attacks against the application.

### Recommended Mitigation

1. Never deploy the `.git` directory to a public web server.
2. Configure the web server to deny access to hidden directories and files.
3. Disable public directory listing.
4. Use a secure deployment process that only publishes required production files.
5. Regularly perform security assessments to identify exposed files.

---

## 6. Flag

**Challenge Answer:**

```text
THM{byt3_l0tus_n3v3r_f0rg3ts}
```

---

## 7. Conclusion

In conclusion, this challenge demonstrates how an incorrectly configured web server can expose sensitive development information.

By performing directory enumeration with Gobuster, an exposed Git repository was discovered. Git-Dumper was then used to retrieve the repository and reconstruct the website's source code.

After analysing the recovered files, the hidden flag was found inside README.md.

This challenge improved my understanding of web directory enumeration, information disclosure vulnerabilities, Git repository exposure and basic web application security testing.

---

