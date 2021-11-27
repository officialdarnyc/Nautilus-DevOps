# Creating 3 groups and 15 users in Linux

## Create Groups

Using bash scripting, the groups were created in bulk. Created a file called `grp-script.sh` using the `touch` command.
Opened the file and added the below script:
```
#!/bin/bash

for group in frontend-dev backend-dev ops;
do 
    groupadd -r ${group};
done
```
Saved and closed the file. Using the `chmod` command, granted my user the execute permission to that file.

```
chmod u+x grp-script.sh
```
Ran the newly created script to bulk create the groups.

```
sudo ./grp-script.sh
```
Confirmed the groups were created by viewing the file `/etc/group` using the `cat` command.

## Create Users

Using bash scripting, the 15 users were created altogether. A file called `userlist.txt` was created and all the usernames for the 15 different users were saved in that file. The bash script called `users-script.sh` was also created and the below scripts were added and saved to the file:
```
#!/bin/bash

# location of the txt file of usernames
# userfile=/home/daniel/userlist.txt

# extracting usernames from the file one-by-one
username=$(cat /home/daniel/userlist.txt | tr 'A-Z'  'a-z')

# running loop  to add users
for user in $username
do
    # adding users '$user' is a variable that changes usernames accordingly in txt file.
    useradd $user
    echo $user:$user"@1234" | sudo chpasswd
done

echo "$(wc -l < /home/daniel/userlist.txt) users have been created"
```
Using the `chmod` command, granted my user the execute permission to that file.

```
chmod u+x users-script.sh
```
Ran the users-script.sh script to bulk create the 15 users and assigned unique passwords to each user in the format (`username@1234`).

```
sudo ./users-script.sh
```
By checking the `/etc/passwd` file, I was able to confirm all the users got created.

```
cut -d: -f1 /etc/passwd
```