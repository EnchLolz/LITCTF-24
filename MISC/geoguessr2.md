# geoguessr2

Category: MISC

Points: 113

Solves: 414

>Our history class recently took a walk on a field trip here. Where is this? Flag format: Use three decimal points of precision and round. The last digit of the first coordinate is EVEN, and the last digit of the second coordinate is ODD.

### Solution

![Chal](/images/geoguessr2.jpg)

We can do a simple reverse image search:

![Reverse image search](/images/geoguessr2rev.png)

Looks like the first image matches up, we can read `Capt John Parker` on the stone and the twitter post starts with `Capt John`:

![X Post](/images/geoguessr2Xpost.png)

Ok nice we found the guy, appearently at Lexington's `Old Burying Ground`. We pinpoint on [google maps](https://www.google.com/maps/place/Old+Burying+Ground,+Lexington,+MA+02421/@42.4504132,-71.2333532,55m/data=!3m1!1e3!4m6!3m5!1s0x89e39dd184e3dc1b:0xd65c44b4295a155f!8m2!3d42.4504269!4d-71.2333139!16s%2Fg%2F11bvtcgn93?entry=ttu) and scroll through photos taken there:

![Stone](/images/geoguessr2maps.png)

We found it.

### Flag

```LITCTF{42.450,-71.233}```