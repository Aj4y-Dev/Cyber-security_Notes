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

```