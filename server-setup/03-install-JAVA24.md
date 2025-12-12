# Java 21 Installation Guide for Ubuntu 24.04 VPS

## 1. Update System
```bash
sudo apt update && sudo apt upgrade -y
```

## 2. Install OpenJDK 21
```bash
sudo apt install openjdk-21-jdk -y
```

## 3. Verify Installation
```bash
java --version
javac --version
```
Expected: `openjdk 21.x.x`[1][2]

## 4. Find Exact Java Path
```bash
ls -la /usr/lib/jvm/
```
Note the folder name (usually `java-21-openjdk-amd64`)

## 5. Set JAVA_HOME System-Wide
```bash
sudo nano /etc/environment
```

**Add this line** (on NEW line after PATH):
```
JAVA_HOME="/usr/lib/jvm/java-21-openjdk-amd64"
```

**Final file should look like:**
```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin"
JAVA_HOME="/usr/lib/jvm/java-21-openjdk-amd64"
```

Save: `Ctrl+O`, `Enter`, `Ctrl+X`

## 6. Apply Changes & Verify
```bash
source /etc/environment
echo $JAVA_HOME
$JAVA_HOME/bin/java --version
```

## 7. Test Complete Setup
```bash
java -version
javac -version
echo $JAVA_HOME
which java
```

## Troubleshooting
- **Wrong path?** Run `ls -la /usr/lib/jvm/` and use exact folder name
- **Not working?** Logout/login or reboot: `sudo reboot`
- **Multiple Java versions?** `sudo update-alternatives --config java`

**All done!** Java 21 is now ready for your VPS applications.[2][3][1]

[1](https://www.rosehosting.com/blog/how-to-install-java-21-on-ubuntu-24-04/)
[2](https://www.server-world.info/en/note?os=Ubuntu_24.04&p=java&f=4)
[3](https://geekrewind.com/how-to-install-openjdk-21-on-ubuntu-24-04/)
