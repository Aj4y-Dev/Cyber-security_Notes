## SSH `authorized_keys` – Summary Notes

### What is `authorized_keys`?

- `~/.ssh/authorized_keys` stores **public SSH keys** allowed to log in **without a password**.
- Each line = **one allowed user/key**.
- Removing a key immediately **revokes access**.

Secure Permissions (Required by SSH)

```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chown -R ajay:ajay ~/.ssh
```

**Meaning:**

- `700 ~/.ssh` → only owner can access the `.ssh` directory
- `600 authorized_keys` → only owner can read/write keys
- Prevents other users from seeing or copying SSH keys
- SSH will **reject login** if permissions are weaker

Prevent Anyone from Modifying Keys (Strong Protection)

```
sudo chattr +i ~/.ssh/authorized_keys
```

**Effect:**

- File becomes **immutable**
- Cannot be edited, deleted, or appended
- Even **root** cannot modify it
- SSH can still read → login works

To edit later:

```
sudo chattr -i ~/.ssh/authorized_keys
```

### What is NOT Possible

- Cannot hide `authorized_keys` from **yourself** or **sshd**
- Cannot make it unreadable (`chmod 000`) → SSH login will break

Final Secure State

```
~/.ssh           → drwx------
authorized_keys → -rw------- (immutable)
```

