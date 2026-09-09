# DevOps Series — Day 1 Notes
## Linux & DevOps Fundamentals

> **Source:** Day 1 transcript — *Master Linux for DevOps FREE Series: DevOps Engineers should Know This (Day 1)*

---

# 1. How Does the Internet Work?

## Basic Idea

Internet communication can be visualized as:

```text
Your Device
    ↓
Internet Service Provider (ISP)
    ↓
Internet Network / Fiber
    ↓
Data Center
    ↓
Server
    ↓
Requested Data
    ↓
Your Device
```

## Data Center

A **Data Center** is a physical location where many computers/servers are kept.

Servers in a data center can:

- Store data
- Process data
- Run applications
- Provide services
- Transfer/serve data

## ISP

**ISP = Internet Service Provider**

An ISP provides internet connectivity to users.

---

# 2. What is a Server?

### Definition

> **A server is a computer/system whose job is to serve information or services to clients.**

Basic model:

```text
Client → Request → Server
Client ← Response ← Server
```

## Examples of Servers

| Server | Purpose |
|---|---|
| Email Server | Handles email-related services |
| File Server | Stores/provides files |
| Database Server | Handles database-related operations |
| Application Server | Runs application/business logic |
| Web Server | Serves web/static content |

---

# 3. Client vs Server

## Client

A **client** is a device/application that requests information or a service from a server.

Examples:

- Laptop
- Mobile phone
- Browser
- Application making a request

## Server

A **server** provides information or services to clients.

### Easy Memory Trick

```text
Client → Consumes/Requests
Server → Provides/Serves
```

Example:

```text
Browser (Client)
       ↓
     Request
       ↓
     Server
       ↓
    Response
       ↓
Browser (Client)
```

---

# 4. Domain Name & IP Address

When you type:

```text
youtube.com
```

`youtube.com` is a **domain name**.

Computers/network communication ultimately uses IP addresses to identify network destinations.

Conceptually:

```text
Domain Name
     ↓
    DNS
     ↓
IP Address
     ↓
Server
```

## DNS

**DNS = Domain Name System**

DNS maps/resolves domain names to IP addresses.

### Remember

```text
Domain Name → Human-friendly name
IP Address  → Network address
DNS         → Domain → IP resolution
```

---

# 5. Web Server vs Application Server

This is an important DevOps concept.

## Web Server

A web server generally serves **static content**.

Examples:

- HTML
- Images
- CSS
- JavaScript/static files

**Nginx** is mentioned as an example of a web server.

## Application Server

An application server generally handles **dynamic content/application logic**.

It may perform:

- Computation
- Calculations
- Business logic
- Dynamic data generation

Examples mentioned:

- Django application
- Node.js application

## Difference

| Web Server | Application Server |
|---|---|
| Mainly serves static content | Mainly handles dynamic/application logic |
| HTML, images, files | Computation and business logic |
| Example: Nginx | Examples: Django/Node.js application |

### Memory Trick

> **Web Server = Serve files**
>
> **Application Server = Run application logic**

---

# 6. Types of Applications

The video broadly discusses:

1. Standalone Application
2. Web Application

---

## 6.1 Standalone Application

A standalone application can operate without requiring a collection of external supporting services.

Examples discussed conceptually:

- Airport feedback machine
- Simple machine that performs an operation after a coin is inserted

Basic idea:

```text
Application
     ↓
Runs independently
```

It does not necessarily require:

- Database server
- Email server
- Cache
- Other external services

---

## 6.2 Web Application

Examples:

- Instagram
- YouTube

A web application can involve multiple supporting components/services.

Example architecture:

```text
                  ┌── Database Server
                  │
Client → Web/App → Application Server
                  │
                  ├── Email Server
                  │
                  └── Other Services
```

A web application may involve:

- Frontend
- Backend
- Database
- Application server
- Email server
- Cloud services
- Other supporting services

---

# 7. Application Support & Maintenance

Applications continuously run, so problems can occur.

Examples:

- Application crash
- Server connection failure
- Email service unavailable
- Network issue
- Dependency failure

## DevOps Troubleshooting Mindset

A DevOps engineer should understand the different components and identify **which layer/component is causing the problem**.

Conceptually:

```text
Application
     ↓
Operating System
     ↓
Server
     ↓
Network
     ↓
Other Services
```

When an issue occurs, ask:

- Is the problem in the application?
- Is the server unavailable?
- Is there a network problem?
- Is a dependency unavailable?
- Is another service failing?

---

# 8. What is an Operating System?

### Definition

> **An Operating System (OS) is a large software/program that helps applications run on a computer.**

Examples:

- Windows
- Linux
- macOS

Basic architecture:

```text
Applications
      ↓
Operating System
      ↓
Hardware
```

The OS acts as an important layer between applications and hardware.

---

# 9. Why Linux for DevOps?

Linux is heavily relevant to DevOps and server environments.

The video discusses Linux in the context of:

- Development
- Networking
- Terminal usage
- Programming
- Scripting

## Linux Characteristics Mentioned

- Open source
- Multitasking
- Security-focused
- Community contribution

## Linux Distributions / Flavors

Examples mentioned:

- Ubuntu
- Kali Linux

### DevOps Perspective

Think of Linux as:

```text
Linux
  ↓
Servers
  ↓
Terminal
  ↓
Commands
  ↓
Processes
  ↓
Networking
  ↓
Automation / Scripting
```

---

# 10. Linux vs Windows — Video Perspective

## Windows

The video presents Windows as:

- Commercially licensed
- Popular for general desktop usage
- Commonly used for entertainment/general-purpose tasks
- Also capable of supporting coding

## Linux

The video emphasizes Linux for:

- Development
- Networking
- Terminal
- Programming
- Scripting
- Server environments

### Important

For a DevOps student:

> **Do not think of Linux only as an operating system. Think of it as an important server, terminal, networking, scripting, and automation environment.**

---

# 11. How Can You Practice Linux?

The video gives several options.

## Option 1 — WSL

**WSL = Windows Subsystem for Linux**

Allows you to use a Linux environment on Windows.

## Option 2 — VirtualBox

Conceptually:

```text
Windows
   ↓
VirtualBox
   ↓
Virtual Machine
   ↓
Ubuntu Linux
```

## Option 3 — Cloud VM

Cloud platforms can provide virtual machines.

Examples:

- AWS
- Azure
- GCP

## Option 4 — Vagrant

Vagrant is mentioned as a tool for working with virtual machine environments.

## Option 5 — Docker

Docker is also briefly mentioned as another approach involving Linux-based environments.

---

# 12. Remote Server Access

DevOps engineers often need to access servers remotely.

Basic model:

```text
Your Laptop
     │
     │ Internet
     ↓
Remote Server
```

## RDP

**RDP = Remote Desktop Protocol**

Used for remote desktop access.

## SSH

**SSH = Secure Shell**

Used to securely access a remote system/server.

Example:

```text
Your Computer
      ↓
     SSH
      ↓
Cloud Linux Server
```

Once connected, you can work with the remote Linux environment from your own computer.

---

# 13. Linux Architecture

This is one of the most important concepts from Day 1.

Basic architecture:

```text
Applications
      ↓
    Shell
      ↓
    Kernel
      ↓
   Hardware
```

## Application

Applications are programs that users interact with or that perform specific tasks.

## Shell

The shell provides an interface for interacting with the Linux system using commands.

## Kernel

The kernel is the core component that interacts with system hardware/resources.

## Hardware

Examples:

- CPU
- RAM
- Disk
- Camera
- Printer
- Scanner

---

# 14. What is Shell?

### Definition

> **Shell is an interface through which you interact with the Linux system/kernel using commands.**

Conceptually:

```text
You
 ↓
Shell
 ↓
Command
 ↓
Kernel
 ↓
Filesystem / Hardware
```

Example:

```bash
mkdir test
```

The shell interprets the command and interacts with the underlying system.

## Important

> **Shell ≠ Kernel**

They are different components.

---

# 15. What is Kernel?

### Definition

> **The kernel is the core component of the operating system that interacts with hardware and manages system resources.**

Basic flow:

```text
Applications
      ↓
    Shell
      ↓
    Kernel
      ↓
   Hardware
```

Examples of resources/hardware involved:

- CPU
- Memory
- Disk
- Devices

For Linux, this is the **Linux kernel**.

---

# 16. Application → Shell → Kernel → Hardware

Remember this flow.

Example:

```text
Application
     ↓
Shell / Interface
     ↓
Kernel
     ↓
Hardware
```

The application initiates an operation, the system interfaces handle the request, and the kernel interacts with the required hardware/resources.

### ⭐ Must Remember

```text
Application
      ↓
    Shell
      ↓
    Kernel
      ↓
   Hardware
```

---

# 17. What is a Boot Loader?

When a computer starts, a boot process is responsible for starting the operating system.

A **boot loader** helps load/start the operating system.

Basic flow:

```text
Computer ON
    ↓
Boot Process
    ↓
Boot Loader
    ↓
OS / Kernel starts
    ↓
System Processes
    ↓
System Ready
```

---

# 18. GRUB

### GRUB

**GRUB = GNU GRUB**

GRUB is mentioned as a Linux boot loader.

### Interview Question

**Q: Name a boot loader used with Linux.**

**Answer: GRUB**

---

# 19. Desktop Environment

A desktop environment provides a graphical user environment.

It can include:

- Icons
- Menus
- Graphical applications
- Terminal access

It is the graphical layer through which users can interact with the operating system.

---

# 20. Linux File System

Linux uses a hierarchical file system.

Basic structure:

```text
/
├── home
├── usr
├── bin
├── etc
├── var
├── tmp
└── ...
```

The `/` directory is the top-level/root of the Linux filesystem hierarchy.

---

# 21. Important Linux Directories

## `/home`

Contains users' personal/home directories.

Example:

```text
/home/user
```

## `/usr`

An important hierarchy containing system/user-related programs and resources.

## `/bin`

Contains binary/executable programs in the traditional Linux filesystem layout.

## `/etc`

Contains system configuration-related files.

## `/var`

Contains variable data.

Logs are commonly found under:

```text
/var/log
```

## `/tmp`

Used for temporary files.

### Quick Table

| Directory | Basic Purpose |
|---|---|
| `/` | Root of filesystem |
| `/home` | User home directories |
| `/usr` | Programs/resources |
| `/bin` | Executable programs |
| `/etc` | Configuration |
| `/var` | Variable data/logs |
| `/tmp` | Temporary files |

---

# 22. Why Linux Has a File System Hierarchy

Linux organizes files/directories into standard locations.

Example:

```text
Logs
 ↓
/var/log
```

If you need to inspect logs, `/var/log` is an important location to know.

### DevOps Importance

Logs are extremely important for troubleshooting.

Example:

```text
Application Issue
       ↓
Check Logs
       ↓
Find Error
       ↓
Troubleshoot
```

Therefore, understanding Linux filesystem locations is essential for DevOps.

---

# 23. `cd` Command

### `cd` = Change Directory

Example:

```bash
cd /var
```

This moves you to the `/var` directory.

Another example:

```bash
cd /var/log
```

moves into the logs directory.

---

# 24. What is a Process?

### Definition

> **A process is a running instance of a program.**

Example:

```text
Program
   ↓
Running
   ↓
Process
```

A Linux system can have many processes running at the same time.

---

# 25. PID — Process ID

Every process has an identifier.

### PID = Process ID

Example:

```text
PID 1
PID 2
PID 3
...
```

PID is used to identify a particular process.

### Remember

> **PID = Process ID**

---

# 26. PID 1

PID 1 has special importance in Linux.

It is associated with the first/main userspace process started during system boot and has an important role in process management.

Conceptually:

```text
System Boot
    ↓
First Userspace Process
    ↓
PID 1
```

### Interview Questions

**Q: What is PID?**

Process ID.

**Q: Why is PID 1 important?**

It is the first/main userspace process started during boot and has special process-management responsibilities.

---

# 27. Process States

The video discusses several process states.

## 1. Running

The process is currently executing.

```text
Process → RUNNING
```

## 2. Sleeping

The process is waiting/not actively executing.

```text
Process → SLEEPING
```

## 3. Terminated

The process has finished execution.

```text
Process → TERMINATED
```

## 4. Zombie

A zombie process is a process that has exited but remains as an entry until it is properly reaped.

### Revision Table

| State | Basic Meaning |
|---|---|
| Running | Process is executing |
| Sleeping | Process is waiting |
| Terminated | Process has ended |
| Zombie | Process exited but remains as an entry until reaped |

---

# 28. Basic Linux Monitoring Commands

Day 1 introduces several commands. Detailed command practice is expected in later sessions.

## `top`

Used to inspect running processes and system activity.

```bash
top
```

Useful for observing:

- Processes
- CPU-related activity
- Process states

## `free`

Used to view memory/RAM information.

```bash
free
```

## `df -h`

Used to view disk usage in a human-readable form.

```bash
df -h
```

## `cd`

Changes the current directory.

```bash
cd /var
```

## `mkdir`

Creates a directory.

```bash
mkdir test
```

### Quick Cheat Sheet

```text
top       → Processes / CPU activity
free      → Memory/RAM information
df -h     → Disk usage
cd        → Change directory
mkdir     → Create directory
```

---

# 29. Cloud & Virtual Machine

The video introduces cloud computing practically through AWS.

Conceptually:

```text
Physical Data Center
        ↓
Physical Servers
        ↓
Virtualized Resources
        ↓
Cloud Services
```

Cloud platforms allow you to create virtual machines/compute resources remotely.

---

# 30. AWS EC2

### EC2

**EC2 = Elastic Compute Cloud**

EC2 is used to create cloud compute instances/virtual servers.

Basic flow:

```text
AWS
 ↓
EC2
 ↓
Virtual Machine / Instance
 ↓
Ubuntu Linux
 ↓
DevOps Practice
```

---

# 31. AWS EC2 Instance

An EC2 instance can act as your remote Linux server.

Conceptually:

```text
AWS Cloud
   ↓
EC2
   ↓
Instance
   ↓
Ubuntu
   ↓
Linux Server
```

You can later connect to this server and practice Linux commands.

---

# 32. Key Pair

While creating an EC2 instance, the video mentions creating/selecting a **key pair**.

A key pair is important for authentication/access to the instance.

---

# 33. Why Ubuntu on EC2?

The video prepares an Ubuntu Linux instance for practicing Linux and DevOps concepts in upcoming sessions.

The purpose is to have a real remote Linux environment available for practice.

---

# 34. BIG PICTURE — Day 1

Connect the concepts together:

```text
                         INTERNET
                            │
                            ↓
                         CLIENT
                    (Laptop / Phone)
                            │
                         Request
                            │
                            ↓
                          DNS
                  Domain → IP resolution
                            │
                            ↓
                     WEB SERVER
                  (Static Content)
                            │
                            ↓
                  APPLICATION SERVER
                   (Dynamic Logic)
                            │
                            ↓
                     DATABASE SERVER
                            │
                            ↓
                         Response
                            │
                            ↓
                         CLIENT
```

Inside the server:

```text
                 LINUX SERVER
                      │
                      ↓
                  Applications
                      │
                      ↓
                    Shell
                      │
                      ↓
                    Kernel
                      │
                      ↓
                   Hardware
```

During boot:

```text
Computer ON
     ↓
Boot Process
     ↓
Boot Loader (GRUB)
     ↓
Linux Kernel
     ↓
PID 1 / System Processes
     ↓
Services
     ↓
User
```

---

# 35. Day 1 — Must Remember

## Internet

- Internet uses networks/fiber infrastructure.
- Data centers contain many computing systems/servers.
- ISP provides internet connectivity.
- Client requests information/services.
- Server provides information/services.

## Web

- Domain name = human-friendly website name.
- DNS resolves domain names to IP addresses.
- Web server generally serves static content.
- Application server generally handles dynamic/application logic.

## Applications

- Standalone application → comparatively independent.
- Web application → can involve multiple supporting services/components.

## Linux

- Linux is important in server/DevOps environments.
- Linux is open source.
- Linux supports terminal, networking, programming and scripting workflows.
- Ubuntu and Kali Linux are distributions mentioned in the video.

## Architecture

```text
Application
    ↓
Shell
    ↓
Kernel
    ↓
Hardware
```

## Boot

- Boot loader helps start the operating system.
- GRUB is a Linux boot loader.

## Filesystem

Know:

```text
/home
/usr
/bin
/etc
/var
/tmp
```

## Processes

- Process = running program instance.
- PID = Process ID.
- PID 1 is special.
- States discussed: Running, Sleeping, Terminated, Zombie.

## Commands

```bash
cd
mkdir
top
free
df -h
```

## Remote Access

```text
RDP → Remote Desktop Protocol
SSH → Secure Shell
```

## Cloud

```text
AWS
 ↓
EC2
 ↓
Instance
 ↓
Ubuntu/Linux
```

---

# 36. Day 1 Interview Questions

Use these as your self-test.

1. What is a server?
2. What is a client?
3. What is the difference between a client and server?
4. What is a data center?
5. What is an ISP?
6. How does data travel over the internet?
7. What is a domain name?
8. What is DNS?
9. What is the difference between a web server and application server?
10. What is static data/content?
11. What is dynamic data/content?
12. What is a standalone application?
13. What is a web application?
14. Why should a DevOps engineer understand application architecture?
15. What is application support and maintenance?
16. What is an operating system?
17. Why is Linux important for DevOps?
18. What is a Linux distribution?
19. What is a shell?
20. What is a kernel?
21. What is the difference between shell and kernel?
22. What is a boot loader?
23. What is GRUB?
24. What is Linux filesystem hierarchy?
25. What is `/home`?
26. What is `/etc`?
27. What is `/var`?
28. Where are logs commonly found?
29. What is a process?
30. What is PID?
31. What is PID 1?
32. What are common process states?
33. What does `top` do?
34. What does `free` do?
35. What does `df -h` do?
36. What does `cd` do?
37. What is SSH?
38. What is RDP?
39. What is AWS EC2?
40. What is an EC2 instance?

---

# 37. Day 1 Practical Task

The Day 1 practical goal is to prepare a Linux environment for upcoming sessions.

## Step 1 — Choose a Linux Environment

Use one of:

```text
WSL
VirtualBox
AWS EC2
```

## Step 2 — Start Ubuntu/Linux

Open the terminal.

## Step 3 — Practice Basic Commands

```bash
pwd
cd /
ls
cd /var
ls
cd /var/log
ls
free
df -h
top
```

> `ls` and `clear` are previewed for the next session, where Linux commands are covered in more detail.

---

# 🧠 Final Mental Model

The most important idea from Day 1 is to connect everything:

```text
Client
   ↓
Internet
   ↓
DNS
   ↓
Server
   ↓
Linux
   ↓
Application / Web Server
   ↓
Processes
   ↓
Shell
   ↓
Kernel
   ↓
Hardware
```

And from the DevOps perspective:

```text
Application
     ↓
Server
     ↓
Linux
     ↓
Processes
     ↓
Logs
     ↓
Networking
     ↓
Cloud
     ↓
Automation
```

> **Day 1 is about building the mental foundation.**
>
> Don't just memorize definitions. Understand how **Internet → Client → Server → Linux → Application → Process → Kernel → Hardware** connects together.

---

## Day 1 Completion Checklist

- [] I understand client vs server
- [ ] I understand what a data center is
- [ ] I understand ISP
- [ ] I understand domain name and DNS
- [ ] I understand web server vs application server
- [ ] I understand standalone vs web applications
- [ ] I understand why Linux matters for DevOps
- [ ] I understand Linux architecture
- [ ] I understand shell vs kernel
- [ ] I understand boot loader and GRUB
- [ ] I know the important Linux directories
- [ ] I understand processes and PID
- [ ] I know basic process states
- [ ] I know `top`, `free`, `df -h`, `cd`, `mkdir`
- [ ] I understand SSH and RDP
- [ ] I understand AWS EC2 at a basic level
- [ ] I have a Linux environment ready for Day 2
