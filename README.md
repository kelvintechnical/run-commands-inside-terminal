# Lab 22-1g — Run Basic Commands Inside the Container

**RHCSA EX200 Lab** | Part of [Linux Ops Mastery](https://github.com/kelvintechnical/linux-ops-mastery)

---

## 📋 Scenario

You have launched `mycontainer` and have a bash shell inside it. Your task is to run a set of basic commands to confirm the container environment is isolated from the host, verify the OS, and explore the container filesystem.

---

## 🎯 Requirements

1. Confirm you are inside the container (not the host)
2. Verify the OS is UBI9
3. Explore the container's filesystem
4. Confirm network isolation with `hostname`

---

## 🧠 Key Concepts

| Command | Purpose |
|---------|---------|
| `whoami` | Confirm running user inside container |
| `hostname` | Shows container ID — NOT the host's hostname |
| `cat /etc/os-release` | Confirms UBI9 OS inside the container |
| `ls /` | Explore root filesystem inside the container |
| `echo` | Basic output test — confirms shell is functional |
| `exit` | Exit the container shell and return to host |

---

## 🔬 Steps

> ✅ You should already be inside the container from Lab 22-1f.  
> Your prompt looks like: `[root@a3f91bc72d10 /]#`

### Step 1 — Confirm your user inside the container

```bash
whoami
```

**Expected output:**
```
root
```

> Inside a rootless Podman container, the process runs as `root` inside the container namespace — but it maps to `conadm` on the host. This is a key security feature of rootless containers.

---

### Step 2 — Check the hostname

```bash
hostname
```

**Expected output:**
```
a3f91bc72d10
```

> The hostname is the **container ID**, not your machine's hostname. This confirms isolation.

---

### Step 3 — Verify the OS

```bash
cat /etc/os-release
```

**Expected output:**
```
NAME="Red Hat Enterprise Linux"
VERSION="9.x (Plow)"
ID="rhel"
...
```

> This confirms you're running UBI9 inside the container regardless of what distro runs on your host.

---

### Step 4 — Explore the filesystem

```bash
ls /
```

**Expected output:**
```
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

> The container has its own complete filesystem, isolated from the host.

---

### Step 5 — Run a basic echo test

```bash
echo "Hello from inside the container"
```

**Expected output:**
```
Hello from inside the container
```

---

### Step 6 — Exit the container

```bash
exit
```

**Expected output:**
```
[conadm@node1 ~]$
```

> You are now back on the host. The container stopped because `/bin/bash` (the main process) exited.

---

## 🔍 Container vs Host Comparison

| What you check | Inside Container | On Host |
|----------------|-----------------|---------|
| `whoami` | `root` | `conadm` |
| `hostname` | Container ID (hash) | Your machine hostname |
| `cat /etc/os-release` | UBI9 / RHEL | Your host OS (Fedora, Rocky, etc.) |
| `ls /` | Container filesystem | Host filesystem |

---

## ⚠️ Pitfalls

- **`exit` stops the container** — because bash was the main process; the container is not "running in the background"
- **root inside ≠ root outside** — rootless Podman maps container root to your unprivileged host user; no actual root access on host
- **Missing commands** — UBI9 is a minimal image; commands like `ping`, `curl`, or `vim` may not be installed inside

---

## 🎓 Exam Tip

> The RHCSA exam may ask you to "verify the container is running the correct image." The correct answer is `cat /etc/os-release` from inside the container — not `podman images` from the host.

---

## ✅ Lab Checklist

- [ ] `whoami` returns `root` inside container
- [ ] `hostname` shows container ID hash (not host hostname)
- [ ] `cat /etc/os-release` confirms UBI9
- [ ] `ls /` shows isolated container filesystem
- [ ] `exit` returns to host prompt as conadm
- [ ] Ready to verify port mapping from host (Lab 22-1h)

---

## 🔗 Series Navigation

| Lab | Description |
|-----|-------------|
| [22-1a](https://github.com/kelvintechnical/lab-22-1a) | Create conadm user |
| [22-1b](https://github.com/kelvintechnical/lab-22-1b) | Grant conadm full sudo rights |
| [22-1c](https://github.com/kelvintechnical/lab-22-1c) | Verify sudo access |
| [22-1d](https://github.com/kelvintechnical/lab-22-1d) | Inspect ubi9 remotely with skopeo |
| [22-1e](https://github.com/kelvintechnical/lab-22-1e) | Pull ubi9 image with podman |
| [22-1f](https://github.com/kelvintechnical/lab-22-1f) | Launch container with -it + port map 80:8080 |
| **22-1g** | **Run basic commands inside container** ← you are here |
| [22-1h](https://github.com/kelvintechnical/lab-22-1h) | Verify port mapping from host |

---

## 👤 Author

**Kelvin R. Tobias** — [kelvinintech.com](https://kelvinintech.com) | [GitHub](https://github.com/kelvintechnical) | [LinkedIn](https://www.linkedin.com/in/kelvin-r-tobias-211949219)
