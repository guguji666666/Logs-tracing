# Use screen to run tcpdump in backend

To install `screen` and manage sessions, you can use package managers like `apt` (for Debian-based systems) or `yum` (for Red Hat-based systems). Here's how you can install `screen` and manage sessions:

### Installation of screen

#### For Debian-based systems (e.g., Ubuntu):
```bash
sudo apt update
sudo apt install screen
```

#### For Red Hat-based systems (e.g., CentOS):
```bash
sudo yum install screen
```

### Usage:

#### Starting a `screen` session:
```bash
screen
```

#### If you want to start tcodump 25226, then
```bash
sudo tcpdump -i any udp port 25226 -X
```


#### Detaching from a `screen` session:
Press `Ctrl + A`, then `Ctrl + D`


#### List existing `screen` sessions:
```bash
screen -ls
```


#### Reattaching to a `screen` session:
```bash
screen -r [session_id]
```
Replace `[session_id]` with the ID of the `screen` session you want to reattach to.

#### Killing a `screen` session:
First, detach from the session if you're currently attached to it (`Ctrl + A`, then `Ctrl + D`). Then, use the following command to terminate a `screen` session:
```bash
screen -X -S [session_id] quit
```
Replace `[session_id]` with the ID of the `screen` session you want to terminate.

#### Creating a named `screen` session:
```bash
screen -S [session_name]
```
Replace `[session_name]` with the name you want to give to the `screen` session.

These commands will help you install `screen`, manage sessions, and perform various operations such as starting, detaching, reattaching, listing, and killing `screen` sessions.
