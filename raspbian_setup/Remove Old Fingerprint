# Remove old finger print from pi

If you use multiple pi's on the same IP or is you have formatted your pi when trying to SSH into the device you may be told 

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:cyFtWenGc0po/NnwXTInNtQ4rnkkCP91Hs0jKyU+6uc.
Please contact your system administrator.
Add correct host key in /Users/sammottley/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /Users/sammottley/.ssh/known_hosts:6
ECDSA host key for 130.88.237.42 has changed and you have requested strict checking.
Host key verification failed.
```

To correct this issue run the below on your host PC (REMEMBER to change the IP to your IP address)

```bash
ssh-keygen -R 130.88.237.42
```

