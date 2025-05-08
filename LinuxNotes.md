# Why Should a developer or devops engineer learn linux

- Linux is the backbone of modern IT infrastructure, and learning it provides a competitive edge for developers and DevOps engineers.
- Most Servers Run on Linux
- Popular DevOps tools like Docker, Kubernetes, Ansible, Terraform, and Jenkins run seamlessly on Linux.
- Major cloud providers (AWS, Azure, GCP) extensively use Linux-based VMs and services
- Linux Servers are highly Secure
- Performance 

# Linux Operating System (OS)

- Linux is a community-driven OS, not owned by any single company.
- Linux is free and open-source, allowing customization and flexibility.
- Linux is highly secure, eliminating the need for antivirus software.
- Linux is primarily CLI-based (Command Line Interface), offering powerful control over the system.
- Linux is widely used in businesses for efficient server management and enterprise solutions.

# Linux History

- Linux OS was developed by Linus Torvalds.
- Linus Torvalds identified several limitations and challenges in the Unix OS.
- He discovered an OS that aligned with his vision, known as Minix OS.
- Using Minix OS as a reference, he modified its code and introduced a new operating system—Linux OS.
- The name Linux comes from: (Li)nus + Mi(nux) = Linux.

# Linux Distributions

Linus Torvalds made the Linux OS source code freely available to the public. Various companies and communities have since modified this source code to meet their specific requirements, releasing their own versions known as Linux distributions or flavors. As of now, there are over 600 active Linux distributions in the market.

Examples include:
- Amazon Linux
- Ubuntu
- Red Hat
- Debian
- Kali
- Fedora

Each of these distributions offers unique features and caters to different user needs, contributing to the rich diversity of the Linux ecosystem.

# How to setup Linux Machine?

**Approach 1:** Install Linux OS Directly on Your System  
1. Download a Linux distribution (e.g., Ubuntu, Fedora, Debian).  
2. Create a bootable USB drive and install it as the primary OS.  

**Approach 2:** Use VirtualBox to Run Linux as a Virtual Machine  
1. Install Oracle VirtualBox or VMware Workstation on your system.  
2. Create a new virtual machine and install Linux as a guest OS.  

**Approach 3:** Deploy a Free Linux VM on AWS Cloud  
1. Sign up for an AWS Free Tier account.  
Note: [Youtube Video to create free AWS Account](https://youtu.be/KBb3-w6hyp0?si=Q3JGWwRKT4rLfeQI)  
2. Launch an EC2 instance with a Linux-based AMI (Amazon Machine Image).  
3. Enjoy 750 free hours per month for up to 1 year under the free tier.  

# Linux Commands Mastery

| Command | Description |
|---------|-------------|
| `whoami` | Displays the currently logged-in username |
| `pwd` | Shows the present working directory |
| `date` | Prints today's date and time |
| `cal` | Displays the calendar for the current month |
| `cal 2025` | Shows the calendar for the entire year 2025 |
| `clear` | Clears the terminal screen |
| `mkdir` | Create a new directory (folder) |
| `rmdir` | Remove an empty directory |
| `rm -rf <dir>` | Delete a non-empty directory and its contents |
| `ls` | List files and directories |
| `ls -l` | Display files in detailed format |
| `touch` | Create empty files |
| `cd` | Change directory |
| `rm` | Delete a file or directory |
| `mv` | Rename or move a file/directory |
| `cat` | Create/view/append files |
| `tac` | Display file content in reverse |
| `cp` | Copy files |
| `head` | Display first lines of a file |
| `tail` | Display last lines of a file |
| `grep` | Search for patterns in files |
| `wc` | Word count command |

# Text Editors in Linux

- **vi (Visual Editor)** is the default text editor in Linux systems.
- The `vi` editor operates in **three modes**:
  1. **Command Mode** - Default mode when opening a file
  2. **Insert Mode** - Press `i` to enter text editing
  3. **Escape Mode** - Press `Esc` to exit insert mode

**Save changes and exit**: `:wq`  
**Exit without saving**: `:q!`

# File Permissions in Linux

Permissions are represented using 9 characters: `rwxrwxrwx`  
- First 3: Owner permissions  
- Middle 3: Group permissions  
- Last 3: Others permissions  

**Changing permissions with `chmod`**:
```bash
chmod u+x file.txt      # Add execute for user
chmod g-w file.txt      # Remove write for group
chmod 755 file.txt      # Numeric permissions (rwxr-xr-x)