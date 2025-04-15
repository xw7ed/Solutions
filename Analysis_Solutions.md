# 🔐 Security Log Analysis Challenge

## 🧠 Challenge Description

Create a pipeline that identifies the **Top 5 IP addresses** with failed SSH login attempts from `/var/log/auth.log`, showing:

1. ✅ **Count of attempts per IP**
2. 🔽 **Sorted from most to least attempts**
3. 💡 **Display only the IP addresses (not full log lines)**

**Bonus:** Also include the **usernames that were attempted** for each IP address.

---

## 🧪 Sample Data Generation

Since most personal computers are not real servers and `/var/log/auth.log` is often empty, you can use the command below to generate a sample log file:

```bash
# Create sample failed SSH attempts with random IPs and usernames
sudo bash -c 'cat > /var/log/auth.log <<EOF
Mar 10 14:23:45 server sshd[12345]: Failed password for root from 203.0.113.45 port 54322 ssh2
Mar 10 14:23:47 server sshd[12345]: Failed password for admin from 203.0.113.45 port 54322 ssh2
Mar 10 14:23:50 server sshd[12345]: Failed password for ubuntu from 198.51.100.22 port 42351 ssh2
Mar 10 14:23:52 server sshd[12345]: Failed password for test from 203.0.113.45 port 54322 ssh2
Mar 10 14:23:55 server sshd[12345]: Failed password for root from 192.0.2.128 port 38422 ssh2
Mar 10 14:23:57 server sshd[12345]: Failed password for admin from 203.0.113.45 port 54322 ssh2
Mar 10 14:24:00 server sshd[12345]: Failed password for guest from 198.51.100.22 port 42351 ssh2
Mar 10 14:24:02 server sshd[12345]: Failed password for root from 172.16.254.3 port 58211 ssh2
Mar 10 14:24:05 server sshd[12345]: Failed password for admin from 203.0.113.45 port 54322 ssh2
Mar 10 14:24:07 server sshd[12345]: Failed password for ubuntu from 10.0.0.27 port 49233 ssh2
Mar 10 14:24:10 server sshd[12345]: Failed password for test from 198.51.100.22 port 42351 ssh2
Mar 10 14:24:12 server sshd[12345]: Failed password for root from 203.0.113.45 port 54322 ssh2
Mar 10 14:24:15 server sshd[12345]: Failed password for admin from 192.0.2.128 port 38422 ssh2
Mar 10 14:24:17 server sshd[12345]: Failed password for guest from 203.0.113.45 port 54322 ssh2
Mar 10 14:24:20 server sshd[12345]: Failed password for root from 198.51.100.22 port 42351 ssh2
EOF'
```

---

## 🔎 Solution

### 📜 Display Only IP Addresses (Top 5):

```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head -n 5 | awk '{print $2}'
```

**Sample Output:**

```
203.0.113.45
198.51.100.22
192.0.2.128
172.16.254.3
10.0.0.27
```

---

### 📊 Count of Attempts per IP (Sorted):

```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head -n 5
```

**Sample Output:**

```
7 203.0.113.45
4 198.51.100.22
2 192.0.2.128
1 172.16.254.3
1 10.0.0.27
```

---

## 📈 SSH Failed Login Analysis - Top Offending IPs

Below are the **Top 5 IP addresses** with the highest number of failed SSH login attempts, along with the usernames that were used:

| IP Address        | Attempts | Usernames                        |
|------------------|----------|----------------------------------|
| 203.0.113.45     | 7        | admin, root, test, guest         |
| 198.51.100.22    | 4        | ubuntu, test, root, guest        |
| 192.0.2.128      | 2        | root, admin                      |
| 172.16.254.3     | 1        | root                             |
| 10.0.0.27        | 1        | ubuntu                           |

---

> ✅ Generated and analyzed by **Abdulwahed (xw7ed)**

