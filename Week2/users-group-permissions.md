## Managing ownership and groups

In my test directory, display the current owner and group associated with the Resources directory and the files.

How do I display permission, owners, and groups?

```
ls -l
```

The `ls -l` command displays directory contents in long format. The long format contains both permissions and ownership. You can see that the user account that created the resources also owns those resources. The group association is also that user's primary group.

How do I change the user/owner associated with file1?

```
chown user02 file1
```

How do I change the group associated with file1?

```
chown :groupA file1
```

How do I change the owner and group at the same time for file2?

```
chown user02:groupA file2
```

There is a specific `chgrp` command, but I prefer only to memorize one command (`chown`) and apply it to both functions (user and group associations) rather than `chown` for the user and then have to recall `chgrp` for the group.

So how do I use `chgrp` command?

```
chgrp groupB file1
```

How do I change the user/group for a directory and all of its contents?

```
chown -R user01:groupA Resources
```

The above task provides a **recursive** configuration. Technically, recursive commands are repeated on each specified object. Effectively, recursive means "this and everything in it." In the above example, you are configuring the related user/group for the Resources directory and everything in it. Without the `-R` option, you would only affect the Resources directory itself, but not its contents.

## Managing permissions in Linux system

The _change mode_ or `chmod` command is used in managing permissions. The syntax is straight-forward:

```
chmod permissions <resource-name>
```

Here are two examples of manipulating permissions for file2:

```
chmod 740 file2
chmod u=rwx,g=r,o-rwx file2
```

But wait! Those appear to be radically different examples (they're not, actually).

Let's discuss the 2 modes of specifying file permissions. The Absolute/Numeric mode and the Symbolic mode.

### The Absolute/Numeric Mode

In this mode, each access level (read, write, execute) has an octal value:

- `4` for read permission
- `2` for write permission
- `1` for execute permission

This gives us 4 combinations:

- `0` no permissions
- `1` can execute
- `2` can write
- `3` can write, execute
- `4` can read
- `5` can read, execute
- `6` can read, write
- `7` can read, write and execute

Each identity (user, group, others) has a position:

- _user_ positioned at the first or left-most end
- _group_ positioned at the middle
- _others_ positioned at the last or right-most end

The absolute mode syntax states the desired permissions from left to right.

We use them in pairs of 3, to set the permissions of all the 3 identities altogether:

```
chmod 777 file1
chmod 755 file2
chmod 644 file3
```

### Symbolic Mode

Symbolic mode uses more symbols, but the symbols are simpler to understand.

- `r` for read permission
- `w` for write permission
- `x` for execute permission

Each identity also has a symbol:

- `u` for user
- `g` for group
- `o` for others

An additional symbol used to specify for all identities is `a` which stands for _all_.

Also in this mode, several operators are used to manipulate the permissions:

- `+` to grant a level of access
- `-` to remove a level of access
- `=` to set a level of access

To remove the read permissions from others for file2 by using symbolic mode

```
chmod o-r file2
```

To grant the read and write permissions to the group for file2

```
chmod g+rw file2
```

To set permissions for a directory and all of its contents

```
chmod -R o=rwx,g+rw,o-rwx Resources
```

Thanks for reading! :+1: