![[Pasted image 20240616213012.png]]

Exiftool is a pretty advanced too that can read, write and change exif data on images and videos. 

to use this tool all we need to do is, 

![[Pasted image 20240616213715.png]]

if you just use the command and point to the image however it will be messy and noithing is catagoriesed, to catagoise i use the command

```
exiftool -a -u -g1 your_image.jpg
```
![[dog.txt]]

we can learn a lot from exif data, like GPS Cords:
![[Pasted image 20240616214102.png]]

And what device took the photo. 
![[Pasted image 20240616214156.png]]

Along with a lot more information that could be deemed useful.

