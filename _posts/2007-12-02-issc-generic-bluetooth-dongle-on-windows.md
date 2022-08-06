---
title: "ISSC Generic Bluetooth Dongle on Windows (XP, Vista)"
date: 2007-12-02
---

For some time now I bought a _generic_ ISSC bluetooth dongle. I've seen that many people have problems with this dongle when trying to install its driver on Windows, since there is none.

After some research about how Windows driver works, I came with a simple fix that works for both Windows XP and Windows Vista (tested). Locate the `bth.inf` file (in `C:\Windows\System32`) and make some modifications:

On the `[Manufacturer]` tab, add this line:

```inf
ISSC=ISSC, NTx86...1
```

Next, add these lines below the `[Manufacturer]` tab:

```inf
[ISSC.NTx86...1]
ISSC Generic Bluetooth=  BthUsb, USB\Vid_1131&Pid_1001&REV_0373
```

That's it! You can do it inside notepad. There is a special note for Windows Vista. You can't modify the file from where it is, so the solution is to copy it (and all other files inside its folder) to another directory, then modify it there and when Windows asks for a manual driver, point it to that folder. Unfortunately I think I can't upload the modified file here due to copyright restrictions.

