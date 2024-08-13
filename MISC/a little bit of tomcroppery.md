# a little bit of tomcroppery

Category: MISC

Points: 118

Solves: 303

>Once you crop an image, the cropped part is gone... right???

### Solution

![Cropped](/images/cropped.png)

Running binwalk on the image we can see an uncropped version saved:

![Binwalk](/images/cropbinwalk.png)

Extracting this image we can see the uncropped version \:D

![Uncropped](/images/cropbinwalk6DB72.png)


### Flag

```LITCTF{4cr0p41yp5e_15_k1nd_0f_c001_j9g0s}```