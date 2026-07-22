# 🔐 Linux File Permissions — Practice Guide

A complete, self-testing reference for `chmod`, `chown`, `umask`, and real-world DevOps permission troubleshooting.

Every question is followed by a collapsible **"View Answer"** section — click it to reveal the solution. Try answering on your own first!

---

## 📌 Quick Note: Default Permissions & `umask`

You mentioned wanting every newly created file/folder to default to `777`, and using a recursive `chmod` on an existing tree. A couple of corrections and clarifications on that:

- **Your command had a small syntax issue.** The recursive flag is a lowercase `-R`, not `+R`:
  ```bash
  # ❌ Incorrect
  chmod +R u+r project/

  # ✅ Correct
  chmod -R u+r project/
  ```
  If your real goal is "give everyone full permission on everything inside `project/`", use:
  ```bash
  chmod -R 777 project/
  ```

- **Why new files/folders are never `777` by default:** Linux doesn't create files with full permissions out of the box — it starts from a maximum ("base") permission and then **subtracts** whatever bits are set in your `umask`.
  - New **files** start at a base of `666` (rw-rw-rw-) — files aren't executable by default.
  - New **directories** start at a base of `777` (rwxrwxrwx).
  - The final permission = base − umask.

- **Check your current umask:**
  ```bash
  umask        # numeric form, e.g. 0022
  umask -S     # symbolic form, e.g. u=rwx,g=rx,o=rx
  ```
  With the common default `umask 022`:
  - New file → `666 - 022 = 644` (rw-r--r--)
  - New directory → `777 - 022 = 755` (rwxr-xr-x)

- **To make every new file/folder `777` by default**, set the umask to `000` (⚠️ not recommended for real systems — see security note below):
  ```bash
  umask 000
  ```
  Add this line to `~/.bashrc` (per-user) or `/etc/profile` (system-wide) to make it permanent.

- **⚠️ Security warning:** A `umask` of `000` (or `chmod 777` on real files) means **every user on the system** can read, write, and execute/enter every new file and folder — including other logged-in users and any compromised process. This is almost always the wrong fix. If the real problem is "another user/group can't access my files," the correct fix is usually:
  ```bash
  chmod -R 775 project/        # owner + group full, others read/execute only
  # or, better — manage access via groups:
  chown -R :developers project/
  chmod -R 2775 project/       # setgid so new files inherit the group too
  ```

- **`umask` only affects newly created files.** It has no effect on files that already exist — for those, you must run `chmod` (recursively, if needed) as shown in Question 30.

---

# 🟢 Easy Level

### Q1. Show the permissions of the file `notes.txt`.

<details>
<summary>View Answer</summary>

```bash
ls -l notes.txt
```
This prints something like `-rw-r--r-- 1 user group 1024 Jul 22 10:00 notes.txt`, where the first 10 characters encode file type + owner/group/other permissions.
</details>

---

### Q2. Create a file named `demo.txt`.

<details>
<summary>View Answer</summary>

```bash
touch demo.txt
```
</details>

---

### Q3. Remove execute permission for everyone from `script.sh`.

<details>
<summary>View Answer</summary>

```bash
chmod a-x script.sh
# or
chmod -x script.sh
```
</details>

---

### Q4. Give execute permission only to the owner of `script.sh`.

<details>
<summary>View Answer</summary>

```bash
chmod u+x script.sh
```
</details>

---

### Q5. Make `data.txt` readable and writable by everyone.

<details>
<summary>View Answer</summary>

```bash
chmod a+rw data.txt
# or numerically
chmod 666 data.txt
```
</details>

---

### Q6. Give only read permission to everyone for `report.txt`.

<details>
<summary>View Answer</summary>

```bash
chmod 444 report.txt
# or
chmod a=r report.txt
```
</details>

---

### Q7. Create a directory named `project`.

<details>
<summary>View Answer</summary>

```bash
mkdir project
```
</details>

---

### Q8. Show detailed permissions of all files in the current directory.

<details>
<summary>View Answer</summary>

```bash
ls -la
```
The `-a` flag also shows hidden (dotfile) entries.
</details>

---

### Q9. Find out who owns `demo.txt`.

<details>
<summary>View Answer</summary>

```bash
ls -l demo.txt
# or specifically
stat -c "%U %G" demo.txt
```
The second column of `ls -l` output is the owner; the third is the group.
</details>

---

### Q10. Display the numeric (octal) permissions of `script.sh` by interpreting the output of `ls -l`.

<details>
<summary>View Answer</summary>

If `ls -l script.sh` shows `-rwxr-xr--`, convert each triplet (owner/group/other) using r=4, w=2, x=1:
- Owner `rwx` = 4+2+1 = **7**
- Group `r-x` = 4+0+1 = **5**
- Other `r--` = 4+0+0 = **4**

So the numeric permission is **754**. You can also get it directly with:
```bash
stat -c "%a" script.sh
```
</details>

---

# 🟡 Medium Level

### Q11. Change permissions: Owner → rwx, Group → r-x, Others → r.

<details>
<summary>View Answer</summary>

```bash
chmod 754 script.sh
# or symbolically
chmod u=rwx,g=rx,o=r script.sh
```
</details>

---

### Q12. Set the permissions of `report.txt` to 644.

<details>
<summary>View Answer</summary>

```bash
chmod 644 report.txt
```
</details>

---

### Q13. Set the permissions of `backup.sh` to 755.

<details>
<summary>View Answer</summary>

```bash
chmod 755 backup.sh
```
</details>

---

### Q14. Set the permissions of `secret.txt` to 600.

<details>
<summary>View Answer</summary>

```bash
chmod 600 secret.txt
```
</details>

---

### Q15. Give write permission only to the group for `project.txt`.

<details>
<summary>View Answer</summary>

```bash
chmod g+w project.txt
```
</details>

---

### Q16. Remove write permission from others for `notes.txt`.

<details>
<summary>View Answer</summary>

```bash
chmod o-w notes.txt
```
</details>

---

### Q17. Change the owner of `file.txt` to user `ubuntu`.

<details>
<summary>View Answer</summary>

```bash
sudo chown ubuntu file.txt
```
</details>

---

### Q18. Change the group of `file.txt` to `developers`.

<details>
<summary>View Answer</summary>

```bash
sudo chgrp developers file.txt
# or
sudo chown :developers file.txt
```
</details>

---

### Q19. Change both owner and group in one command (owner → ubuntu, group → developers).

<details>
<summary>View Answer</summary>

```bash
sudo chown ubuntu:developers file.txt
```
</details>

---

### Q20. Recursively give execute permission to the owner for every file/directory inside `project`.

<details>
<summary>View Answer</summary>

```bash
chmod -R u+x project/
```
Note the flag is a capital `-R` (recursive), not `+R`.
</details>

---

# 🔴 Hard Level

### Q21. Create directory `website` with Owner: rwx, Group: r-x, Others: none.

<details>
<summary>View Answer</summary>

```bash
mkdir website
chmod 750 website
```
</details>

---

### Q22. `deploy.sh` shows `-rw-r--r--` and gives "Permission denied" when run. Fix it.

<details>
<summary>View Answer</summary>

The script isn't executable — none of the permission triplets have `x`. Add execute permission:
```bash
chmod +x deploy.sh
# then run it
./deploy.sh
```
</details>

---

### Q23. Convert `rwxr-xr--` into numeric form.

<details>
<summary>View Answer</summary>

- Owner `rwx` = 7
- Group `r-x` = 5
- Other `r--` = 4

**Answer: 754**
</details>

---

### Q24. Convert 754 into symbolic permissions.

<details>
<summary>View Answer</summary>

**Answer: `rwxr-xr--`**
(7=rwx, 5=r-x, 4=r--)
</details>

---

### Q25. Convert 640 into symbolic permissions.

<details>
<summary>View Answer</summary>

**Answer: `rw-r-----`**
(6=rw-, 4=r--, 0=---)
</details>

---

### Q26. A file has permission 777. Why is this insecure?

<details>
<summary>View Answer</summary>

`777` gives **every user on the system** — owner, group, and others — full read, write, and execute access. Any user (or a compromised process running as another user) can modify, delete, or execute the file. For scripts, this also means anyone could inject malicious code and run it. Sensitive data, configuration files, and scripts should always follow the **principle of least privilege** — grant only the access actually needed.
</details>

---

### Q27. A file has permission 600. Who can read/modify it? Can others access it?

<details>
<summary>View Answer</summary>

`600` = `rw-------`
- **Read:** only the owner.
- **Modify (write):** only the owner.
- **Group & others:** no access at all (no r, w, or x) — they cannot even list its contents if it were a directory. This is the standard permission for private/sensitive files like SSH private keys (`id_rsa`).
</details>

---

### Q28. Nginx can't read `index.html` (permission is 600). Fix it.

<details>
<summary>View Answer</summary>

Nginx runs as its own user (commonly `www-data` or `nginx`), which is neither the file owner nor typically in the owner's private group, so `600` blocks it entirely. Fix by allowing at least read access to others (or the web server's group):
```bash
chmod 644 index.html
```
For directories in the path leading to the file, ensure they have execute (`x`) permission too, e.g. `chmod 755 /var/www/html`.
</details>

---

### Q29. A shell script copied from Windows has `-rw-r--r--`. Make it executable.

<details>
<summary>View Answer</summary>

```bash
chmod +x script.sh
```
Bonus: scripts copied from Windows often also have `\r\n` line endings that break the shebang line. Fix with:
```bash
sed -i 's/\r$//' script.sh
```
</details>

---

### Q30. Change every file and directory in a project (500 files) to 755, recursively.

<details>
<summary>View Answer</summary>

```bash
chmod -R 755 project/
```
Note: this sets the **same** mode on files and directories alike. A more correct approach that keeps files non-executable (unless they already were) uses `find`:
```bash
find project/ -type d -exec chmod 755 {} \;   # directories → 755
find project/ -type f -exec chmod 644 {} \;   # files → 644
```
</details>

---

# 🔥 Interview Challenge

Given `-rwxr-x---`, answer:

<details>
<summary>View Answer</summary>

1. **Who can read?** Owner and group (both have `r`).
2. **Who can write?** Only the owner.
3. **Who can execute?** Owner and group (both have `x`).
4. **Can "others" access it?** No — the last triplet is `---`, no permissions at all.
5. **Octal permission:** `750` (owner `rwx`=7, group `r-x`=5, other `---`=0).
</details>

---

# 🧠 Scenario-Based Questions (Real DevOps)

### Scenario 1: Nginx site shows 403 Forbidden.

<details>
<summary>View Answer</summary>

Check and fix permissions along the entire path, not just the file:
```bash
ls -l /var/www/html/index.html
namei -l /var/www/html/index.html   # shows permissions of every parent directory
sudo chmod 644 /var/www/html/index.html
sudo chmod 755 /var/www/html
```
Also verify the Nginx worker user (`www-data`) has read access — check `nginx.conf` for the `user` directive, and confirm SELinux/AppArmor isn't blocking it if enabled.
</details>

---

### Scenario 2: Cloned Git repo's deploy script says "Permission denied".

<details>
<summary>View Answer</summary>

```bash
ls -l deploy.sh          # confirm no execute bit
chmod +x deploy.sh       # add execute permission
git update-index --chmod=+x deploy.sh   # persist the executable bit in git itself
```
</details>

---

### Scenario 3: "I can read the file but cannot edit it."

<details>
<summary>View Answer</summary>

```bash
ls -l file.txt          # check current permissions and owner
whoami                  # confirm which user you are
groups                  # confirm which groups you belong to
```
If you're not the owner and not in the owning group, ask the owner/admin to either add write access to the group, add you to the group, or change ownership:
```bash
chmod g+w file.txt
sudo usermod -aG groupname yourusername
```
</details>

---

### Scenario 4: Let another user edit a file without changing ownership.

<details>
<summary>View Answer</summary>

Add them to the file's group and grant the group write access:
```bash
sudo usermod -aG developers otheruser
chmod g+w file.txt
sudo chgrp developers file.txt   # if not already in that group
```
For more granular control (without changing the group), use **ACLs**:
```bash
setfacl -m u:otheruser:rw file.txt
getfacl file.txt   # verify
```
</details>

---

### Scenario 5: Accidentally ran `chmod 777 secret.txt`. Why risky, and what's better?

<details>
<summary>View Answer</summary>

`777` lets **any user on the system** read, modify, or (if it's a script) execute the file — a major exposure for anything sensitive (credentials, keys, configs). Revert to the least-privilege setting appropriate for the file:
```bash
chmod 600 secret.txt   # only the owner can read/write
```
</details>

---

## 💡 Bonus Challenge — Answers

| Required Permission | Numeric | Symbolic (One Possible Command)      |
| -------------------- | ------- | ------------------------------------- |
| `rwxr-xr-x`          | 755     | `chmod u=rwx,g=rx,o=rx file`          |
| `rw-r--r--`          | 644     | `chmod u=rw,g=r,o=r file`             |
| `rw-------`          | 600     | `chmod u=rw,g=,o= file`               |
| `rwx------`          | 700     | `chmod u=rwx,g=,o= file`              |
| `rwxrwxr-x`          | 775     | `chmod u=rwx,g=rwx,o=rx file`         |
| `rwxr-x---`          | 750     | `chmod u=rwx,g=rx,o= file`            |

---

# ⭐ Extra Practice Questions

### Q31. What's the difference between `chmod 777 file` and `chmod -R 777 folder`?

<details>
<summary>View Answer</summary>

`chmod 777 file` changes permissions on a **single file/directory only**. `chmod -R 777 folder` applies the change **recursively** to the folder itself and everything inside it (all subdirectories and files). The `-R` flag is what makes it recursive — without it, only the top-level item is affected.
</details>

---

### Q32. What is the `setgid` bit used for on a shared project directory, and how do you set it?

<details>
<summary>View Answer</summary>

`setgid` on a directory makes every new file/subdirectory created inside it **inherit the parent directory's group** automatically, instead of the creating user's default group — very useful for shared team folders.
```bash
chmod g+s project/
# or numerically (prefix with 2)
chmod 2775 project/
```
</details>

---

### Q33. What does the "sticky bit" do, and where is it commonly used?

<details>
<summary>View Answer</summary>

The sticky bit, when set on a directory, ensures that **only the file's owner** (or root) can delete or rename files inside that directory, even if other users have write access to the directory. It's classically used on `/tmp`:
```bash
chmod +t /tmp
# or numerically (prefix with 1)
chmod 1777 /tmp
```
In `ls -l`, it shows as a `t` at the end of the permission string, e.g. `drwxrwxrwt`.
</details>

---

### Q34. How do you find all files in a directory with world-writable (o+w) permissions — a common security audit task?

<details>
<summary>View Answer</summary>

```bash
find /path/to/dir -type f -perm -o+w
```
`-perm -o+w` matches files where the "others write" bit is set, regardless of the other bits.
</details>

---

### Q35. You want new files inside `/shared` to always be `664` and new directories `775`, without manually chmod-ing each one. How?

<details>
<summary>View Answer</summary>

Set an appropriate `umask` for the session/user creating files there (umask `002` gives files `664` and directories `775` from their `666`/`777` bases):
```bash
umask 002
```
Combine this with `setgid` on the directory (Q32) so everything also shares the same group automatically:
```bash
chmod g+s /shared
```
</details>

---

### Q36. Difference between `chmod` and `chattr`?

<details>
<summary>View Answer</summary>

`chmod` manages the standard **read/write/execute** permission bits for owner/group/others. `chattr` sets **extended filesystem attributes** on ext-family filesystems that override normal permission logic — for example, the immutable attribute:
```bash
sudo chattr +i important.conf
```
This makes the file undeletable and unmodifiable by *anyone*, including root, until the attribute is removed with `chattr -i`. It's a much stronger lock than any `chmod` setting.
</details>

---

### Q37. Why might `chmod 755` on a file still fail with "Operation not permitted"?

<details>
<summary>View Answer</summary>

Common causes:
- You aren't the file's owner and aren't root — only the owner or root can change permissions.
- The filesystem is mounted **read-only** (check with `mount | grep <path>`).
- The file has an immutable attribute set via `chattr +i` (see Q36) — `lsattr file` will reveal an `i` flag.
- You're on a network filesystem (NFS/SMB) with restricted permission handling.
</details>

---

*Practice tip: try answering each question yourself before expanding "View Answer" — recalling the command from memory is what makes it stick.*
