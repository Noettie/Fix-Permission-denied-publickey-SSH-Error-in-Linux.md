
# Fix Permission denied (publickey) SSH Error in Linux
This quick tutorial shows you how to fix ssh error “sign_and_send_pubkey: signing failed: agent refused operation Permission denied (publickey)” on Linux.

If you are trying to connect to the remote server via SSH, you might encounter permission denied error. This error may happen for a number of reasons. And the fix to this issue depends upon the exact reason behind the error.

In my case, I had the public and private keys stored on my Ubuntu 16.04 desktop. After Ubuntu 18.04 release, I decided to upgrade to this newer version. I prefer a fresh install over distribution upgrades.

So, I made a backup of the main folders of my Home directory including the .ssh folder that had public and private keys on an external disk.

Once installed, I enabled SSH on Ubuntu 18.04 and restored everything including the SSH keys.

Now when I tried to connect to the remote server using ssh, I thought it would work straightaway because I had the same public and private keys.

But it didn’t work. SSH gave me this error:

sign_and_send_pubkey: signing failed: agent refused operation
root@xxx.xxx.xxx.xx: Permission denied (publickey).
If you are in a similar situation where you copied your SSH keys from another source, let me show you how to fix this SSH error.

Correct file permissions on ~/.ssh folder and its content
As a rule of thumb, you can set the following permissions on the ssh directory and its files (private keys, public keys, known_hosts, ssh config file etc)

Element	Permission
.ssh directory	700 ((drwx------)
public keys	644 (-rw-r--r--)
private keys	600 (-rw-------)
authorized_keys	600 (-rw-------)
known_hosts	600 (-rw-------)
config	600 (-rw-------)
You may not have all the files but you must have public and private keys here.

Now let's see how to change the file permission on the ssh keys and other files.

Fixing Permission denied (publickey) error
So the problem lies with file permissions here. You see, when I copied the files, the USB was in Microsoft’s FAT file format. This file doesn’t support the UNIX/Linux file permissions.

And hence the permissions on the copied ssh keys were changed to 777.

For SSH, the file permissions are too open. It’s simply not allowed to have 777 permissions on the public or private keys. And this is why SSH refused connection here.

ls -l .ssh
-rwxrwxrwx 1 abhishek abhishek 1766 Nov 12  2017 id_rsa
-rwxrwxrwx 1 abhishek abhishek  398 Nov 12  2017 id_rsa.pub
-rwxrwxrwx 1 abhishek abhishek 4214 Sep 21 21:39 known_hosts
The private key should have read and write permissions only for the user and no other permissions for the group and others.

You should change the permission using the chmod command:

chmod 600 ~/.ssh/id_rsa
Similarly, the public key shouldn’t have write and execute permissions for group and other.

chmod 644 ~/.ssh/id_rsa.pub
Now that you have put the correct permissions, you can connect to ssh again. At this time,  it will ask your admin password to unlock the keys. Enter your admin password and you should be good to go.

This also taught me a lesson that copy-pasting files is a bad idea and a proper backup should be made else all the files will have the dangerous 777 permissions on them. I had to recursively change the file permissions on the entire Home directory and trust me, it wasn’t a pretty thing to do.

As I said earlier, there can be various reasons for this error. For the issue related to open file permission, this fix should help you fix the Permission denied (publickey) error with SSH. If you are interested, you can read more about SSH basics.

Let me know in the comment section if the fix worked for you or not. Also suggest your opinion on copying ssh keys on other computers.
