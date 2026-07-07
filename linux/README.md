# Top 20 Commands Every Beginner Should Master First

1. `pwd`
2. `ls`
3. `cd`
4. `mkdir`
5. `touch`
6. `cp`
7. `mv`
8. `rm`
9. `cat`
10. `less`
11. `find`
12. `grep`
13. `chmod`
14. `ps`
15. `top`
16. `ping`
17. `curl`
18. `ssh`
19. `df`
20. `sudo`

**Shell essentials (pipe & chaining):** `|`, `>`, `>>`, `&&`, `||` — see [Phase 12](#phase-12-pipes-redirection--chaining)

# Linux Commands for Beginners

## Phase 1: Navigation (ফাইল ও ডিরেক্টরি ঘোরা)

### pwd

বর্তমান directory দেখায়।

```bash
pwd
```

**Output:**

```
/home/arman
```

### ls

ফাইল ও folder দেখায়।

```bash
ls
ls -l
ls -la
```

**Example:**

```bash
ls /etc
```

### cd

Directory change করে।

```bash
cd Documents
cd ..
cd ~
cd /
```

## Phase 2: File & Directory Management

### mkdir

নতুন directory তৈরি করে।

```bash
mkdir projects
mkdir -p projects/vue/chatapp
```

### touch

ফাইল তৈরি করে।

```bash
touch app.js
touch index.html style.css
```

### cp

Copy করে।

```bash
cp file.txt backup.txt
```

**Directory copy:**

```bash
cp -r project project_backup
```

### mv

Move বা rename করে।

**Rename:**

```bash
mv file.txt file_old.txt
```

**Move:**

```bash
mv file.txt Documents/
```

### rm

Delete করে।

```bash
rm file.txt
```

**Directory delete:**

```bash
rm -r project
```

**Force delete:**

```bash
rm -rf project
```

> ⚠️ খুব সাবধানে ব্যবহার করতে হবে।

## Phase 3: Viewing Files

### cat

```bash
cat notes.txt
```

### less

```bash
less large.log
```

**Navigation:**

- Space = Next page
- `q` = Quit

### head

```bash
head file.txt
head -20 file.txt
```

### tail

```bash
tail file.txt
```

**Live log:**

```bash
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
```

**Case insensitive:**

```bash
grep -i "error" app.log
```

**Recursive:**

```bash
grep -r "database" .
```

## Phase 5: Permissions

### chmod

Permission change করে।

```bash
chmod 755 script.sh
```

**Executable:**

```bash
chmod +x script.sh
```

### chown

Owner change করে।

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

**Modern alternative:**

```bash
htop
```

### kill

```bash
kill 1234
```

**Force:**

```bash
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
```

**Download:**

```bash
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

**Disk usage:**

```bash
df -h
```

### du

**Folder size:**

```bash
du -sh Downloads
```

### mount

**Mounted drives:**

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

## Phase 10: Package Management

**Ubuntu/Debian:**

### apt

**Update:**

```bash
sudo apt update
```

**Upgrade:**

```bash
sudo apt upgrade
```

**Install:**

```bash
sudo apt install nginx
```

**Remove:**

```bash
sudo apt remove nginx
```

## Phase 11: Power User Commands

### history

```bash
history
```

### man

**Manual:**

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

## Phase 12: Pipes, Redirection & Chaining

এক কমান্ডের output অন্য কমান্ডের input হিসেবে পাঠানো, ফাইলে লেখা, বা একাধিক কমান্ড একসাথে চালানো — এগুলো shell-এর সবচেয়ে দরকারী দক্ষতা।

### | (pipe)

এক কমান্ডের output সরাসরি পরের কমান্ডে পাঠায়।

**Error খুঁজে গণনা:**

```bash
grep "error" app.log | wc -l
```

**Output:**

```
12
```

**Log থেকে unique IP দেখা:**

```bash
cat access.log | cut -d' ' -f1 | sort | uniq
```

**ফাইলের বড় লাইনগুলো খুঁজে top 10:**

```bash
cat notes.txt | awk '{ print length, $0 }' | sort -rn | head -10
```

**Process list থেকে node খুঁজা:**

```bash
ps aux | grep node
```

### > (redirect — overwrite)

Output ফাইলে লেখে। আগের content মুছে যায়।

```bash
ls -la > files.txt
```

**Output:**

```
# কোনো output terminal-এ দেখাবে না — সব files.txt-তে যাবে
```

### >> (redirect — append)

Output ফাইলের শেষে যোগ করে। আগের content থাকে।

```bash
echo "Backup done at $(date)" >> backup.log
```

### < (input redirect)

ফাইলের content কমান্ডে input হিসেবে দেয়।

```bash
wc -l < app.log
```

**Output:**

```
1542
```

### 2> (stderr redirect)

Error message আলাদা ফাইলে বা `/dev/null`-এ পাঠায়।

**Error লুকানো:**

```bash
find / -name "*.conf" 2>/dev/null
```

**Error আলাদা ফাইলে:**

```bash
node script.js 2> errors.log
```

### 2>&1 (stderr → stdout)

Error ও normal output একসাথে redirect করে।

```bash
npm install > install.log 2>&1
```

### && (run if success)

আগের কমান্ড সফল হলে পরেরটা চলে।

```bash
mkdir myproject && cd myproject && git init
```

**Build সফল হলে test:**

```bash
npm run build && npm test
```

### || (run if failure)

আগের কমান্ড ব্যর্থ হলে পরেরটা চলে।

```bash
ping -c 1 google.com || echo "No internet connection"
```

### ; (run regardless)

ফলাফল যাই হোক, কমান্ডগুলো একের পর এক চলে।

```bash
cd /tmp ; ls ; pwd
```

### wc

লাইন, শব্দ, অক্ষর গণনা করে। Pipe-এর সাথে খুব কাজে লাগে।

```bash
wc -l app.log
```

**Pipe সহ:**

```bash
grep "warning" app.log | wc -l
```

**Output:**

```
7
```

### sort

লাইন সাজায়।

```bash
sort names.txt
```

**Reverse (বড় থেকে ছোট):**

```bash
sort -rn numbers.txt
```

**Pipe সহ unique value:**

```bash
cat users.log | cut -d',' -f2 | sort | uniq
```

### uniq

পাশাপাশি duplicate লাইন সরায়। সাধারণত `sort` এর পর ব্যবহার হয়।

```bash
sort access.log | uniq
```

**প্রতিটি লাইন কতবার এসেছে:**

```bash
sort access.log | uniq -c | sort -rn
```

### tee

Output terminal-এ দেখায় এবং একই সাথে ফাইলেও লেখে।

```bash
ls -la | tee files.txt
```

**Append mode:**

```bash
echo "new line" | tee -a files.txt
```

### xargs

Pipe থেকে আসা input নিয়ে আরেক কমান্ড চালায়।

**সব `.js` ফাইলে search:**

```bash
find . -name "*.js" | xargs grep "TODO"
```

**ফাইল delete (সাবধানে):**

```bash
find . -name "*.tmp" | xargs rm
```

### Real-world pipe examples

**সবচেয়ে বড় 5 folder:**

```bash
du -sh * | sort -rh | head -5
```

**Git repo-তে কোন ফাইলে "password" আছে:**

```bash
grep -r "password" . --include="*.js" --include="*.env"
```

**Live log filter:**

```bash
tail -f app.log | grep --line-buffered "ERROR"
```

**HTTP status code count:**

```bash
cat access.log | awk '{print $9}' | sort | uniq -c | sort -rn
```

