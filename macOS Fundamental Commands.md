---

## 🧭 **macOS Terminal Course — Level 1: Fundamentals**

### 📌 Module 1: What Is the Terminal?

* It's a **Command-Line Interface (CLI)** where you control your Mac using typed commands instead of the graphical interface (GUI).
* You can open it by:

  * Going to **Launchpad > Other > Terminal**
  * Or using **Spotlight** with `Cmd + Space` and typing “Terminal”

---

### 📌 Module 2: Navigating Files and Folders

| Command          | Description                                        |
| ---------------- | -------------------------------------------------- |
| `pwd`            | Show your current folder (Print Working Directory) |
| `ls`             | List files in current folder                       |
| `cd folder_name` | Change into a folder                               |
| `cd ..`          | Go up one level                                    |
| `cd ~`           | Go to your home folder                             |
| `cd /`           | Go to the root directory                           |

✅ Try this:

```bash
pwd
ls
cd ~
mkdir terminal_test
cd terminal_test
touch file.txt
ls
```

---

### 📌 Module 3: Creating, Deleting, Moving, and Copying Files

| Command                    | Description                      |
| -------------------------- | -------------------------------- |
| `touch file.txt`           | Create an empty file             |
| `mkdir folder`             | Create a folder                  |
| `rm file.txt`              | Delete a file                    |
| `rm -r folder`             | Delete a folder and its contents |
| `mv source destination`    | Move or rename files             |
| `cp source destination`    | Copy a file                      |
| `cp -r folder destination` | Copy a folder                    |

✅ Try this:

```bash
mkdir data
touch data/info.txt
cp data/info.txt copy.txt
mv copy.txt data/renamed.txt
rm data/renamed.txt
```

---

### 📌 Module 4: Viewing File Contents

| Command         | Description             |
| --------------- | ----------------------- |
| `cat file.txt`  | Print all file content  |
| `less file.txt` | View file paginated     |
| `head file.txt` | Show the first 10 lines |
| `tail file.txt` | Show the last 10 lines  |

---

### 📌 Module 5: Managing Processes

| Command       | Description                     |
| ------------- | ------------------------------- |
| `./program &` | Run a program in the background |
| `jobs`        | List background jobs            |
| `fg %1`       | Bring job 1 to the foreground   |
| `kill %1`     | Terminate job 1                 |
| `kill -9 PID` | Force-kill a process by PID     |

---

### 📌 Module 6: Permissions & Executing

| Command              | Description                    |
| -------------------- | ------------------------------ |
| `chmod +x script.sh` | Make a file executable         |
| `./script.sh`        | Run the file                   |
| `which command`      | Show path to a command         |
| `man command`        | Show manual page for a command |

---

### ✅ What’s Next?

Would you like to follow a **daily lesson**, a **weekly module**, or just learn as needed? I can tailor it to your goals—whether you're more into **science/programming**, **system management**, or **everyday productivity**.

Let me know your preferred pace and goals, and we’ll kick it off!
