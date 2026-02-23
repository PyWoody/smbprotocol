This is an opinionated fork of the
[https://github.com/jborean93/smbprotocol](smbprotocol) library. It is intended for personal use only so expect bugs.

The motivation for the fork was to make `smbclient` work seamlessly with the `os`, `os.path`, and `shutil` modules found in the stdlib.

Methods that manipulate paths will automatically correct path seperators to `\` to make path operations more predictable. Any method that is not re-implemented will automatically fall-back to the stdlib implimentation. Note, this may (will) cause unintended side-effects.

Report any bug you encounter directly to this fork. Do not report any issues to `smbprotocol` as they are not responsible or affiliated with the fork.


## Examples

Using `smbclient` without tracking what methods are implemented

```python3
import smbclient

SMB = r'\\server\share'

for root, dirs, files in smbclient.walk(SMB):
    csvs = (
        smbclient.path.join(root, i)
        for i in files
        if smbclient.path.splitext(f)[1].lower() == '.csv'
    )
    for csv in csvs:
        print(f'Working on {smbclient.path.basename(csv)}')
        # Do something
```

Compared to the default implementation that requires using `os` directly:

```python3
import os
import smbclient

SMB = r'\\server\share'

for root, dirs, files in smbclient.walk(SMB):
    csvs = (
        os.path.join(root, i)
        for i in files
        if os.path.splitext(f)[1].lower() == '.csv'
    )
    for csv in csvs:
        print(f'Working on {os.path.basename(csv)}')
        # Do something
```

And for the truly deranged 

```python3
import smbclient as os

SMB = r'\\server\share'

for root, dirs, files in os.walk(SMB):
    csvs = (
        os.path.join(root, i)
        for i in files
        if os.path.splitext(f)[1].lower() == '.csv'
    )
    for csv in csvs:
        print(f'Working on {os.path.basename(csv)}')
        # Do something
```
