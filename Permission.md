### `umask` Notes (Simple Explanation)

**`umask` = permission remover (a security guard)**

Think of `umask` like a **guard at the gate**:

> When a new file or folder is created, the guard removes some permissions before allowing it.

It **does not give permissions**. It only **blocks/removes permissions**.

---

### Default permissions

When Linux creates something:

* **File starts with:** `666` → `rw-rw-rw-`
* **Directory starts with:** `777` → `rwxrwxrwx`

Then `umask` removes permissions.

---

### Example: Your system

Check:

```bash
umask
```

Output:

```text
0002
```

Meaning:

```
0002
   |
   └── Remove write (w) permission from Others
```

Calculation for a file:

```
Default permission: 666
Umask removal:     -002
                   ----
Final permission:   664
```

Result:

```
-rw-rw-r--
```

Meaning:

```
Owner  → read + write
Group  → read + write
Others → read only
```

---

### For a directory:

```
Default: 777
Umask:  -002
          ---
Result: 775
```

Result:

```
rwxrwxr-x
```

---

### Remember:

```
DEFAULT PERMISSION - UMASK = FINAL PERMISSION
```

Example:

```
666 - 002 = 664   (file)
777 - 002 = 775   (directory)
```

**Guard example:**

```
New file wants:
rw-rw-rw-

Guard (umask 0002):
"Others should not write"

Final:
rw-rw-r--
```

`umask` controls **new files only**.
`chmod` changes permissions of **existing files**.
