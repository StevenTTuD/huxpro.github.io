---
author: StevenTTuD
layout: post
title: EDX Linux Foundation Ch 9：User Environment Section 5 File Permissions
published: true
date: 2014-09-14 10:33
tags:
  - Linux
  - EDX Linux Foundation Course
comments: true

---
##File Ownership
![](https://lh5.googleusercontent.com/5pM1DTFHc1_5F8GpOpCa5rfG7hBk7xk9JN4uPV9UIxk=w1755-h485-no)
#0.File Permission Modes

| rwx:| rwx: |  rwx |
| :---: | :-----: | :----: |
| u:  |  g: |  o |


###rwx
 Files have three kinds of permissions: read (r), write (w), execute (x). These are generally represented as in **rwx**.


###ugo
 u:user/owner
 g:group
 0:others

#1.chmod
```
$ ls -l a_file
-rw-rw-r-- 1 coop coop 1601 Mar 9 15:04 a_file
$ chmod uo+x,g-w a_file
$ ls -l a_file
-rwxr--r-x 1 coop coop 1601 Mar 9 15:04 a_file
```
##This kind of syntax can be difficult to type and remember
so one often uses a shorthand which lets you set all the permissions in one step.

**4** if read permission is desired.
**2** if write permission is desired.
**1** if execute permission is desired.
Thus **7** means read/write/execute, **6** means read/write, and **5** means read/execute.

When you apply this to the chmod command you have to give three digits for each degree of freedom, such as in
```
$ chmod 755 a_file
$ ls -l a_file
-rwxr-xr-x 1 coop coop 1601 Mar 9 15:04 a_file
```

#2.chown
有三個檔案file-1, file-2, temp
```
$ ls -l
total 4
-rw-rw-r--. 1 bob bob 0 Mar 16 19:04 file-1
-rw-rw-r--. 1 bob bob 0 Mar 16 19:04 file-2
drwxrwxr-x. 2 bob bob 4096 Mar 16 19:04 temp
```
對file-1下```chown```改變檔案擁有者為root
```
$ sudo chown root file-1
[sudo] password for bob:
```
下```ls -a```看檔案完整的訊息，發現檔案擁有者已經改變成root。
```
$ ls -l
total 4
-rw-rw-r--. 1 root bob 0 Mar 16 19:04 file-1
-rw-rw-r--. 1 bob bob 0 Mar 16 19:04 file-2
drwxrwxr-x. 2 bob bob 4096 Mar 16 19:04 temp
```

#3.chgrp
續上面的例子：這次對file-2下```chgrp```改變檔案的群組。
```
$ sudo chgrp bin file-2
$ ls -l
total 4
-rw-rw-r--. 1 root bob 0 Mar 16 19:04 file-1
-rw-rw-r--. 1 bob bin 0 Mar 16 19:04 file-2
drwxrwxr-x. 2 bob bob 4096 Mar 16 19:04 temp
```