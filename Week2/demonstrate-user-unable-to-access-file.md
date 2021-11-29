# Demonstrating user(s) in a group cannot access files/folders that belong to another group

To do this demonstration, created 3 separate files using the below command and edited them individually using the `nano` command:

```
touch fedevdoc.txt bedevdoc.txt opsdoc.txt
```
Ran the following command to view the files and the current permissions on each of them.

```
ls -l fedevdoc.txt opsdoc.txt bedevdoc.txt
```
The output:

```
-rw-rw-r-- 1 daniel daniel   0 Nov 28 22:57 bedevdoc.txt
-rw-rw-r-- 1 daniel daniel   0 Nov 28 22:57 fedevdoc.txt
-rw-rw-r-- 1 daniel daniel   0 Nov 28 22:57 opsdoc.txt
```
Assigned each group to its related file.

```
sudo chown :frontend-dev fedevdoc.txt
sudo chown :backend-dev bedevdoc.txt
sudo chown :ops opsdoc.txt
```
Confimed that the assignment worked as expected by running the command `ls -l fedevdoc.txt opsdoc.txt bedevdoc.txt` again, and the output was as follows:

```
-rw-rw-r-- 1 daniel backend-dev  0 Nov 28 22:57 bedevdoc.txt
-rw-rw-r-- 1 daniel frontend-dev 0 Nov 28 22:57 fedevdoc.txt
-rw-rw-r-- 1 daniel ops          0 Nov 28 22:57 opsdoc.txt
```

Each file now belongs to its respective group. Meaning that only members of the group that owns the file can have whatever permissions specified; that is, read and write permission (as demonstrated by the middle three characters rw-). However, all other users outside of the group also have read permissions (as demonstrated by the last three characters r--).

Restricted file access to only members that belonged to the file group by removing the read permission from the `others` identity and also assigned execute permission to the fie group.

```
sudo chmod g+x,o-r bedevdoc.txt
sudo chmod g+x,o-r fedevdoc.txt
sudo chmod g+x,o-r opsdoc.txt
```
Confirmed my changes by running the `ls -l` command again and got the below output.

```
-rw-rwx--- 1 daniel backend-dev  0 Nov 28 22:57 bedevdoc.txt
-rw-rwx--- 1 daniel frontend-dev 0 Nov 28 22:57 fedevdoc.txt
-rw-rwx--- 1 daniel ops          0 Nov 28 22:57 opsdoc.txt
```
Switched user to the `ops5` user and tried viewing the _fedevdoc.txt_ file using the `cat` command but got the below result:

```
cat: fedevdoc.txt: Permission denied
```

Switched to the `fedev5` user and was able to successfully view the content of the _fedevdoc.txt_ file.

```
fedev5@Dan-WorkStation:~$ cat fedevdoc.txt
this is a front-end dev grp file
fedev5@Dan-WorkStation:~$
```

