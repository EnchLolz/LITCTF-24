# jwt-1

Category: WEB

Points: 112

Solves: 470

>I just made a website. Since cookies seem to be a thing of the old days, I updated my authentication! With these modern web technologies, I will never have to deal with sessions again.

### Solution

Classic JWT. After we sign in with a new account we can see we have a JWT cookie set. We can then modify this cookie to give us admin and go back to the website for the flag:

![Bad JWT](/images/jwt1bad.png)

![Good JWT](/images/jwt1good.png)

![Flag](/images/jwt1flag.png)


### Flag

```LITCTF{o0ps_forg0r_To_v3rify_1re4DV9}```