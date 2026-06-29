# Top 20 Commands Every Beginner Should Master

1. pwd
2. ls
3. cd
4. mkdir
5. touch
6. cp
7. mv
8. rm
9. cat
10. less
11. find
12. grep
13. chmod
14. ps
15. top
16. ping
17. curl
18. ssh
19. df
20. sudo

# Linux Commands for Beginners

## Phase 1: Navigation

### pwd
Shows the current directory.

```bash
pwd
```

### ls
Lists files and directories.

```bash
ls
ls -l
ls -la
```

### cd
Changes directory.

```bash
cd Documents
cd ..
cd ~
cd /
```

## Phase 2: File & Directory Management

### mkdir

```bash
mkdir projects
mkdir -p projects/vue/chatapp
```

### touch

```bash
touch app.js
touch index.html style.css
```

### cp

```bash
cp file.txt backup.txt
cp -r project project_backup
```

### mv

```bash
mv file.txt file_old.txt
mv file.txt Documents/
```

### rm

```bash
rm file.txt
rm -r project
rm -rf project
```

## Phase 3: Viewing Files

### cat

```bash
cat notes.txt
```

### less

```bash
less large.log
```

### head

```bash
head file.txt
head -20 file.txt
```

### tail

```bash
tail file.txt
tail -f app.log
```

## Phase 4: Search

### find

```bash
find . -name "*.js"
find /home -name "*.pdf"
```

### grep

```bash
grep "error" app.log
grep -i "error" app.log
grep -r "database" .
```

## Phase 5: Permissions

### chmod

```bash
chmod 755 script.sh
chmod +x script.sh
```

### chown

```bash
sudo chown user:user file.txt
```

## Phase 6: Process Management

### ps

```bash
ps aux
```

### top

```bash
top
```

### htop

```bash
htop
```

### kill

```bash
kill 1234
kill -9 1234
```

## Phase 7: Networking

### ping

```bash
ping google.com
```

### curl

```bash
curl https://api.github.com
curl -O file.zip
```

### wget

```bash
wget https://example.com/file.zip
```

### ssh

```bash
ssh user@192.168.1.10
```

## Phase 8: Disk & Storage

### df

```bash
df -h
```

### du

```bash
du -sh Downloads
```

### mount

```bash
mount
```

## Phase 9: System Information

### uname

```bash
uname -a
```

### hostname

```bash
hostname
```

### whoami

```bash
whoami
```

### uptime

```bash
uptime
```

## Phase 10: Package Management (Ubuntu/Debian)

### apt

```bash
sudo apt update
sudo apt upgrade
sudo apt install nginx
sudo apt remove nginx
```

## Phase 11: Power User Commands

### history

```bash
history
```

### man

```bash
man ls
```

### which

```bash
which node
```

### sudo

```bash
sudo apt update
```

