# Add the 15 users across the 3 Groups

As I had earlier created 3 groups namely: _frontend-dev_, _backend-dev_, and _ops_, and also created 15 users to be added (5 users per group) into the groups, I used the `gpasswd` command to add the 5 users per group. That is:

```
sudo gpasswd -M fedev1,fedev2,fedev3,fedev4,fedev5 frontend-dev
sudo gpasswd -M bedev1,bedev2,bedev3,bedev4,bedev5 backend-dev
sudo gpasswd -M ops1,ops2,ops3,ops4,ops5 ops
```
Using the `getent` command, I was able to verify that the users got added to their respective groups.

```
getent group backend-dev
```
Output: `backend-dev:x:998:bedev1,bedev2,bedev3,bedev4,bedev5`