---
author: Juan Diego Llano Miraval
title: Reporte Analisis Forense
date: 18/05/2024
---

![logo_it_uc3m](Logo-uc3m.jpg)

# Reporte de Data carving - Floppy Disk

Juan Diego Llano Miraval

Fecha: 18/05/2024

## Procedure

The first step was checking the sha256:

![sha](sha.png)

it is correct to say this is the same as the provided, so we mount the disk into autopsy, and got 3:

![autopsy](autop.png)

I also found another document but it was a copy of the second document in the image. From this we can see 2 documents of text, and 1 microsoft excel file. When we checked the first file:

![doc1](doc1.png)

it is just text that is not relevant to the case, but in the metadata we can confirm that Emma Crook was the creator of the file, so she might be the owner of the floppy disk. The second file is similar as the first one, there is no relevant information related to the investigation:


![doc2](doc2.png)

For the third file, it was encrypted:

![xls](xls.png)

we exported the .xls for further analysis. We extracted the sha256 from the excel file before proceeding:

![sha excel](sha_excel.png)

 We confirmed that this is a microsoft excel file and we proceed to open. When we open it requests a password, so it is encrypted:

![excel](excel.png)

We took out the hash of the file with the help of john the ripper:

![hash](hash.png)

And by running a simple analysis (by accident i was checking the hash was correct) john the ripper got the password from a default password list:

![pass](pass.png)

Another strategy I was planning to use, was an incremental brute force attack. We have finally opened the excel file and got: 

![evidence](evidence.png)

This is the evidence requested that Emma Crook was selling information.