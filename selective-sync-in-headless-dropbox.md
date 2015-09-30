# Selective Sync with Headless Dropbox

## Headless Installation

### Daemon and Account

```
cd ~
wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64"|tar xzf -
# wget -O - "https://www.dropbox.com/download?plat=lnx.x86"|tar xzf -
~/.dropbox-dist/dropboxd
```

Then copy the given link in a web browser to connect to a specific account.

### Startup script

Install a startup script to start the daemon at boot time.

## Selective Sync

### Advanced Management Tool

```
wget https://www.dropbox.com/download?dl=packages/dropbox.py -O ~/.dropbox-dist/dropbox.py
chmod a+x ~/.dropbox-dist/dropbox.py
```

### Exclude list

Prepare a file (e.g. `/tmp/exclude-list.txt`) with a list of all directories to exclude, then:

```
~/.dropbox-dist/dropbox.py exclude add `cat /tmp/exclude-list.txt`
```

