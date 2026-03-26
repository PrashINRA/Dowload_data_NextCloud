# Downloading Data from Nextcloud on HPC

A quick reference for downloading shared Nextcloud data, unzipping, organizing, and cleaning up.

---

## 1. Start a tmux session

This keeps the download running even if your SSH connection drops.

```bash
tmux new -s download
```

## 2. Navigate to target directory

```bash
cd /path/to/your/target/directory
```

## 3. Download from Nextcloud

The `?accept=zip` parameter tells Nextcloud to package the shared folder as a single ZIP file.

**Using wget:**

```bash
wget --content-disposition "https://your-nextcloud-link/public.php/dav/files/TOKEN/?accept=zip"
```

**Using curl (fallback):**

```bash
curl -OJ "https://your-nextcloud-link/public.php/dav/files/TOKEN/?accept=zip"
```

If the server does not suggest a filename, force one with:

```bash
curl -o data.zip "https://your-nextcloud-link/public.php/dav/files/TOKEN/?accept=zip"
```

## 4. Unzip the data

Without password:

```bash
unzip *.zip
```

With password:

```bash
unzip -P 'your_password_here' *.zip
```

You can chain download and unzip so it runs automatically:

```bash
wget --content-disposition "LINK" && unzip -P 'your_password_here' *.zip
```

## 5. Organize files

If the unzipped data is buried in nested folders, move files to the desired location:

```bash
# Check what is inside
ls /path/to/nested/folder/

# Move files up
mv /path/to/nested/folder/* /path/to/your/target/directory/

# Remove empty nested directories
rm -r /path/to/top-level-nested-folder/
```

## 6. (Optional) Remove the ZIP file

```bash
rm *.zip
```

## 7. Detach and kill tmux session

```bash
# Detach: press Ctrl+b, then d

# List active sessions
tmux ls

# Kill a specific session
tmux kill-session -t download

# Or kill all sessions
tmux kill-server
```

---

## Useful tmux commands

| Action                     | Command / Shortcut        |
|----------------------------|---------------------------|
| New session                | `tmux new -s name`        |
| Detach                     | `Ctrl+b`, then `d`        |
| List sessions              | `tmux ls`                 |
| Reattach                   | `tmux attach -t name`     |
| New window (same session)  | `Ctrl+b`, then `c`        |
| Switch windows             | `Ctrl+b`, then `n` or `p` |
| Kill session               | `tmux kill-session -t name` |
| Kill all                   | `tmux kill-server`        |
