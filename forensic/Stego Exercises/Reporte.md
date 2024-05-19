---
author: Juan Diego Llano Miraval
title: Reporte Analisis Forense
date: 19/05/2024
---

![logo_it_uc3m](Logo-uc3m.jpg)

# Report for Stego Exercises

Juan Diego Llano Miraval

Fecha: 19/05/2024

## procedure

1. for the first CTF, we download the image:

![doge](doge_stege.png)

Then we get the sha256 to verify it:

![sha1](sha1.png)

With the muted colormap I can start to see some letters:


![1muted](1muted.png)

with the op2 map I can see more clearly the letters:

![1op2](1op2.png)

With a little more of playing with the colors I get:

![final](final.png)

pctf{keep_doge_alive_2014}

2. We verify the sha of the image:

![sha2](sha2.png)

We run a binwalk to verify any file inside, and indeed we can see another image inside:


![2binwalk](2binwalk.png)

I used dd to extract the jpg that I can see thanks to binwalk:

![dd](dd.png)

and after checking the images, one of them is:

![2ans](2ans.png)

GCTF{d474_c4rv1n6_15_345y}

3. We download the image:

![lol](lol.png)

Then we used a stego tool to follow the hints, it is MSB, only one color channel, and top down means columns. From this and trying every channel I got:

![3solve](3solve.png)


flag{MSB_really_sucks}pizzas

4.  From this one we got an audio file. We will use audacity to analyze it:

![spectro](spectro.png)

we cant take too much information from this so we will keep looking. I checked the integrity of the file and I ran a binwalk on the wav file and I got:

![file3](file3.png)

When I analize the files with Deepsound I can see one of the ask me for a password:

![pass3](pass3.png)

by playing a little with the spectrogram with Sonic visualizer I get the following:

![GoodSpectro](Goodspectro.png)

Wit the extracted code: BSXPeQrbG3HrG1in2VrU I was able to access the file with Deepsound:

![decode3](decode3.png)

but the flag is not there:

![doc](doc.png)

when we check this doc file, we found the following information on the metadata:

![flagau](flagau.png)

GCTF{57364n06r4phy_15n7_7h47_h4rd}


5. Now we are given another image to process:

![image_Ziki](image_Ziki.png)

we will check the integrity of the file first:

![5file](5file.png)


we can see now that there is a zip file inside the image, so we will try to retrieve it:


![binwalk5](binwalk5.png)

the pass file contains:

```
You're nearly there.
Password: l0v3z1z1
```
I tried using jphs but it didnt work, all the outputs were just data, nothing to be analyzed:

![jphs](jphs.png)

Using a windows version:

![spseek](jpseek.png)

That zip file contained a .txt file:

![flag5](flag5.png)

GCTF{z1pp3d_17_r34l_71gh7}