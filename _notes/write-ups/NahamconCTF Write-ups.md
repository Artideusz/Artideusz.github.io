---
title: Writeups - NahamconCTF 2020
layout: note
permalink: /notes/writeups/nahamcon-ctf-2020
name: NahamconCTF 2020
grouped_by: write-ups
---
# NahamconCTF Write-ups

This is a combined version of all challenged I completed in NahamconCTF 2020.

## Warmup Category

### **Challenge: CLIsay**
#### Description: cowsay is hiding something from us! Download the file below.

We are given a binary file that has our flag. First I will check the type using the "file" command.

File command output: 
```
    clisay: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=70e60678f1c0b75ea3aae2a4e8e1e8978e3c6fc0, for GNU/Linux 3.2.0, not stripped
```

Source code look:

```
/ Sorry, I'm not allow to reveal any \
\ secrets...                         /
  ----------------------------------
         \   ^__^ 
          \  (oo)\_______
             (__)\       )\/\
                 ||----w |
                 ||     ||
... %s r3Ad_M1nd5}...
```

I used the cowsay command to see if that gives me the flag by chance, but unfortunalety nothing interesting catched my eye. After some time I checked the source code of the file and there appeared the flag!

Solution: The flag was found in the source code of the file.

### **Challenge: Metameme**
#### Description: Hacker memes. So meta. Download the file below.

We are given an image of a meme.

First I will use binwalk to see if something else is hidden behind the image:

Binwalk result:
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
202           0xCA            Unix path: /www.w3.org/1999/02/22-rdf-syntax-ns#'>
292           0x124           Unix path: /purl.org/dc/elements/1.1/'>
```

My solution was to grep the flag from the image.
```
    grep -a "flag" < hackermeme.jpg
```

Another solution was to use exiftool. The creator is the flag.

### **Challenge: Mr Robot**

#### Description: Elliot needs your help. You know what to do. Connect here: http://jh2i.com:50032

We are given a URL with a poster of Mr. Robot with a caption saying: "No rest for the wicked" - Mr. Robot

I downloaded the image and checked it using exiftool and binwalk.

Exiftool results:
```
ExifTool Version Number         : 10.80
File Name                       : poster.jpg
Directory                       : .
File Size                       : 30 kB
File Modification Date/Time     : 2020:06:12 18:37:31+02:00
File Access Date/Time           : 2020:06:12 18:37:41+02:00
File Inode Change Date/Time     : 2020:06:12 18:37:39+02:00
File Permissions                : rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Image Width                     : 368
Image Height                    : 550
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 368x550
Megapixels                      : 0.202
```

Binwalk results:
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
```

Next, I checked the source code of the webpage, but found nothing.

I thought about checking /robots.txt, since the challenge is called Mr. Robot, and that is the solution.

### **Challenge: Pang**
#### Description: This file does not open! Download the file below.

We are given a file that cannot open. Looking at the source we can see the flag and that it is a PNG Image.

exiftool results:
```
ExifTool Version Number         : 10.80
File Name                       : pang
Directory                       : .
File Size                       : 8.4 kB
File Modification Date/Time     : 2020:06:12 21:23:03+02:00
File Access Date/Time           : 2020:06:12 21:23:03+02:00
File Inode Change Date/Time     : 2020:06:12 21:23:03+02:00
File Permissions                : rw-rw-r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 1567
Image Height                    : 70
Bit Depth                       : 8
Color Type                      : Grayscale
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Gamma                           : 2.2
White Point X                   : 0.3127
White Point Y                   : 0.329
Red X                           : 0.64
Red Y                           : 0.33
Green X                         : 0.3
Green Y                         : 0.6
Blue X                          : 0.15
Blue Y                          : 0.06
Background Color                : 0
Modify Date                     : 2020:06:05 14:51:14
Datecreate                      : 2020-06-05T18:51:14-04:00
Datemodify                      : 2020-06-05T18:51:14-04:00
Label                           : flag{wham_bam_thank_you_for_the_flag_maam}
Image Size                      : 1567x70
Megapixels                      : 0.110
```

### **Challenge: UGGC**
#### Description: Become the admin! Connect here: http://jh2i.com:50018

We are greeted with a prompt telling us to choose a username to login. I will use AAABBB.

- POST URL: http://jh2i.com:50018/login
- POST parameters:
    - username: AAABBB

Response: 
```
You are logged in as AAABBB.

Sorry, only admin can see the flag.
```

The cookie given to me is user=NNNOOO, I assume that this is a caesar cipher or ROT-13 encoding.

I will attempt to ROT-13 encode the word "admin" and change the cookie to the value.
```
    echo "admin" | tr a-z n-za-m
```
output: nqzva

I am going to change the cookie by assigning the value to document.cookie and see if that works.

```
    document.cookie = "user=nqzva"
```

Result after refreshing the page:
```
You are logged in as admin.
Congratulations here is your flag!

flag{H4cK_aLL_7H3_C0okI3s}
```

## Web Category

### **Challenge: Agent 95**

#### Description: 
```
They've given you a number, and taken away your name~

Connect here:
http://jh2i.com:50000
```

We are greeted with a webpage containing the following text:
```
You don't look like our agent!
We will only give our flag to our Agent 95! He is still running an old version of Windows...
```

I opened postman, copied the request and changed the User-Agent header to:
```
Mozilla/4.0 (compatible; MSIE 5.5; Windows 95; BCD2000)
```

The result contains the flag.

### **Challenge: Localghost**
#### Description: 
```
BooOooOooOOoo! This spooOoOooky client-side cooOoOode sure is scary! What spoOoOoOoky secrets does he have in stooOoOoOore??

Connect here:
http://jh2i.com:50003

Note, this flag is not in the usual format.
```

Looking at the source code of the webpage, we can see that the ghosts are generated through AJAX requests to the ghost.html page.

ghost.html:
```
<pre id="segment">
    .-.
   .'   `.
   :g g   :
   : o    `.
  :         ``.
 :             `.
:  :         .   `.
:   :          ` . `.
 `.. :            `. ``;
    `:;             `:'
       :              `.
        `.              `.     .
          `'`'`'`---..,___`;.-'
</pre>
```

jquery.jscroll2.js:
```
var _0xbcec=["\x75\x73\x65\x20\x73\x74\x72\x69\x63\x74","\x6A\x73\x63\x72\x6F\x6C\x6C","\x3C\x73\x6D\x61\x6C\x6C\x3E\x4C\x6F\x61\x64\x69\x6E\x67\x2E\x2E\x2E\x3C\x2F\x73\x6D\x61\x6C\x6C\x3E","\x61\x3A\x6C\x61\x73\x74","","\x66\x6C\x61\x67","\x53\x6B\x4E\x55\x52\x6E\x74\x7A\x63\x47\x39\x76\x62\x32\x39\x76\x61\x33\x6C\x66\x5A\x32\x68\x76\x63\x33\x52\x7A\x58\x32\x6C\x75\x58\x33\x4E\x30\x62\x33\x4A\x68\x5A\x32\x56\x39","\x73\x65\x74\x49\x74\x65\x6D","\x6C\x6F\x63\x61\x6C\x53\x74\x6F\x72\x61\x67\x65","\x64\x61\x74\x61","\x66\x75\x6E\x63\x74\x69\x6F\x6E","\x64\x65\x66\x61\x75\x6C\x74\x73","\x65\x78\x74\x65\x6E\x64","\x6F\x76\x65\x72\x66\x6C\x6F\x77\x2D\x79","\x63\x73\x73","\x76\x69\x73\x69\x62\x6C\x65","\x66\x69\x72\x73\x74","\x6E\x65\x78\x74\x53\x65\x6C\x65\x63\x74\x6F\x72","\x66\x69\x6E\x64","\x62\x6F\x64\x79","\x68\x72\x65\x66","\x61\x74\x74\x72","\x20","\x63\x6F\x6E\x74\x65\x6E\x74\x53\x65\x6C\x65\x63\x74\x6F\x72","\x74\x72\x69\x6D","\x73\x72\x63","\x69\x6D\x67","\x66\x69\x6C\x74\x65\x72","\x6C\x6F\x61\x64\x69\x6E\x67\x48\x74\x6D\x6C","\x6C\x65\x6E\x67\x74\x68","\x2E\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x69\x6E\x6E\x65\x72","\x3C\x64\x69\x76\x20\x63\x6C\x61\x73\x73\x3D\x22\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x69\x6E\x6E\x65\x72\x22\x20\x2F\x3E","\x77\x72\x61\x70\x41\x6C\x6C","\x63\x6F\x6E\x74\x65\x6E\x74\x73","\x70\x61\x67\x69\x6E\x67\x53\x65\x6C\x65\x63\x74\x6F\x72","\x68\x69\x64\x65","\x63\x6C\x6F\x73\x65\x73\x74","\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x6E\x65\x78\x74\x2D\x70\x61\x72\x65\x6E\x74","\x61\x64\x64\x43\x6C\x61\x73\x73","\x2E\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x69\x6E\x6E\x65\x72\x2C\x2E\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x61\x64\x64\x65\x64","\x6E\x6F\x74","\x70\x61\x72\x65\x6E\x74","\x3C\x64\x69\x76\x20\x63\x6C\x61\x73\x73\x3D\x22\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x6E\x65\x78\x74\x2D\x70\x61\x72\x65\x6E\x74\x22\x20\x2F\x3E","\x77\x72\x61\x70","\x75\x6E\x77\x72\x61\x70","\x63\x68\x69\x6C\x64\x72\x65\x6E","\x2E\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x61\x64\x64\x65\x64","\x72\x65\x6D\x6F\x76\x65\x44\x61\x74\x61","\x2E\x6A\x73\x63\x72\x6F\x6C\x6C","\x75\x6E\x62\x69\x6E\x64","\x64\x69\x76\x2E\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x69\x6E\x6E\x65\x72","\x62\x6F\x72\x64\x65\x72\x54\x6F\x70\x57\x69\x64\x74\x68","\x70\x61\x64\x64\x69\x6E\x67\x54\x6F\x70","\x73\x63\x72\x6F\x6C\x6C\x54\x6F\x70","\x74\x6F\x70","\x6F\x66\x66\x73\x65\x74","\x68\x65\x69\x67\x68\x74","\x63\x65\x69\x6C","\x77\x61\x69\x74\x69\x6E\x67","\x70\x61\x64\x64\x69\x6E\x67","\x6F\x75\x74\x65\x72\x48\x65\x69\x67\x68\x74","\x69\x6E\x66\x6F","\x6A\x53\x63\x72\x6F\x6C\x6C\x3A","\x66\x72\x6F\x6D\x20\x62\x6F\x74\x74\x6F\x6D\x2E\x20\x4C\x6F\x61\x64\x69\x6E\x67\x20\x6E\x65\x78\x74\x20\x72\x65\x71\x75\x65\x73\x74\x2E\x2E\x2E","\x6E\x65\x78\x74\x48\x72\x65\x66","\x77\x61\x72\x6E","\x6A\x53\x63\x72\x6F\x6C\x6C\x3A\x20\x6E\x65\x78\x74\x53\x65\x6C\x65\x63\x74\x6F\x72\x20\x6E\x6F\x74\x20\x66\x6F\x75\x6E\x64\x20\x2D\x20\x64\x65\x73\x74\x72\x6F\x79\x69\x6E\x67","\x61\x75\x74\x6F\x54\x72\x69\x67\x67\x65\x72","\x61\x75\x74\x6F\x54\x72\x69\x67\x67\x65\x72\x55\x6E\x74\x69\x6C","\x73\x63\x72\x6F\x6C\x6C\x2E\x6A\x73\x63\x72\x6F\x6C\x6C","\x62\x69\x6E\x64","\x63\x6C\x69\x63\x6B\x2E\x6A\x73\x63\x72\x6F\x6C\x6C","\x3C\x64\x69\x76\x20\x63\x6C\x61\x73\x73\x3D\x22\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x6C\x6F\x61\x64\x69\x6E\x67\x22\x3E","\x3C\x2F\x64\x69\x76\x3E","\x68\x74\x6D\x6C","\x6C\x61\x73\x74","\x3C\x64\x69\x76\x20\x63\x6C\x61\x73\x73\x3D\x22\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x61\x64\x64\x65\x64\x22\x20\x2F\x3E","\x61\x70\x70\x65\x6E\x64","\x65\x72\x72\x6F\x72","\x72\x65\x6D\x6F\x76\x65","\x2E\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x6E\x65\x78\x74\x2D\x70\x61\x72\x65\x6E\x74","\x63\x61\x6C\x6C\x62\x61\x63\x6B","\x63\x61\x6C\x6C","\x64\x69\x72","\x6C\x6F\x61\x64","\x64\x69\x76\x2E\x6A\x73\x63\x72\x6F\x6C\x6C\x2D\x61\x64\x64\x65\x64","\x61\x6E\x69\x6D\x61\x74\x65","\x64\x65\x62\x75\x67","\x6F\x62\x6A\x65\x63\x74","\x61\x70\x70\x6C\x79","\x6C\x6F\x67","\x73\x6C\x69\x63\x65","\x70\x72\x6F\x74\x6F\x74\x79\x70\x65","\x66\x6E","\x69\x6E\x69\x74\x69\x61\x6C\x69\x7A\x65\x64","\x65\x61\x63\x68"];(function(_0xe943x1){_0xbcec[0];_0xe943x1[_0xbcec[1]]= {defaults:{debug:false,autoTrigger:true,autoTriggerUntil:false,loadingHtml:_0xbcec[2],padding:0,nextSelector:_0xbcec[3],contentSelector:_0xbcec[4],pagingSelector:_0xbcec[4],callback:false}};window[_0xbcec[8]][_0xbcec[7]](_0xbcec[5],atob(_0xbcec[6]));var _0xe943x2=function(_0xe943x3,_0xe943x4){var _0xe943x5=_0xe943x3[_0xbcec[9]](_0xbcec[1]),_0xe943x6=( typeof _0xe943x4=== _0xbcec[10])?{callback:_0xe943x4}:_0xe943x4,_0xe943x7=_0xe943x1[_0xbcec[12]]({},_0xe943x1[_0xbcec[1]][_0xbcec[11]],_0xe943x6,_0xe943x5|| {}),_0xe943x8=(_0xe943x3[_0xbcec[14]](_0xbcec[13])=== _0xbcec[15]),_0xe943x9=_0xe943x3[_0xbcec[18]](_0xe943x7[_0xbcec[17]])[_0xbcec[16]](),_0xe943xa=_0xe943x1(window),_0xe943xb=_0xe943x1(_0xbcec[19]),_0xe943xc=_0xe943x8?_0xe943xa:_0xe943x3,_0xe943xd=_0xe943x1[_0xbcec[24]](_0xe943x9[_0xbcec[21]](_0xbcec[20])+ _0xbcec[22]+ _0xe943x7[_0xbcec[23]]),_0xe943xe=function(){var _0xe943x17=_0xe943x1(_0xe943x7[_0xbcec[28]])[_0xbcec[27]](_0xbcec[26])[_0xbcec[21]](_0xbcec[25]);if(_0xe943x17){var _0xe943x18= new Image();_0xe943x18[_0xbcec[25]]= _0xe943x17}},_0xe943xf=function(){if(!_0xe943x3[_0xbcec[18]](_0xbcec[30])[_0xbcec[29]]){_0xe943x3[_0xbcec[33]]()[_0xbcec[32]](_0xbcec[31])}},_0xe943x10=function(_0xe943x19){var _0xe943x1a;if(_0xe943x7[_0xbcec[34]]){_0xe943x19[_0xbcec[36]](_0xe943x7[_0xbcec[34]])[_0xbcec[35]]()}else {_0xe943x1a= _0xe943x19[_0xbcec[41]]()[_0xbcec[40]](_0xbcec[39])[_0xbcec[38]](_0xbcec[37])[_0xbcec[35]]();if(!_0xe943x1a[_0xbcec[29]]){_0xe943x19[_0xbcec[43]](_0xbcec[42])[_0xbcec[41]]()[_0xbcec[35]]()}}},_0xe943x11=function(){return _0xe943xc[_0xbcec[49]](_0xbcec[48])[_0xbcec[47]](_0xbcec[1])[_0xbcec[18]](_0xbcec[30])[_0xbcec[45]]()[_0xbcec[44]]()[_0xbcec[27]](_0xbcec[46])[_0xbcec[45]]()[_0xbcec[44]]()},_0xe943x12=function(){_0xe943xf();var _0xe943x1b=_0xe943x3[_0xbcec[18]](_0xbcec[50])[_0xbcec[16]](),_0xe943x1c=_0xe943x3[_0xbcec[9]](_0xbcec[1]),_0xe943x1d=parseInt(_0xe943x3[_0xbcec[14]](_0xbcec[51]),10),_0xe943x1e=isNaN(_0xe943x1d)?0:_0xe943x1d,_0xe943x1f=parseInt(_0xe943x3[_0xbcec[14]](_0xbcec[52]),10)+ _0xe943x1e,_0xe943x20=_0xe943x8?_0xe943xc[_0xbcec[53]]():_0xe943x3[_0xbcec[55]]()[_0xbcec[54]],_0xe943x21=_0xe943x1b[_0xbcec[29]]?_0xe943x1b[_0xbcec[55]]()[_0xbcec[54]]:0,_0xe943x22=Math[_0xbcec[57]](_0xe943x20- _0xe943x21+ _0xe943xc[_0xbcec[56]]()+ _0xe943x1f);if(!_0xe943x1c[_0xbcec[58]]&& _0xe943x22+ _0xe943x7[_0xbcec[59]]>= _0xe943x1b[_0xbcec[60]]()){_0xe943x16(_0xbcec[61],_0xbcec[62],_0xe943x1b[_0xbcec[60]]()- _0xe943x22,_0xbcec[63]);return _0xe943x15()}},_0xe943x13=function(_0xe943x1c){_0xe943x1c= _0xe943x1c|| _0xe943x3[_0xbcec[9]](_0xbcec[1]);if(!_0xe943x1c||  !_0xe943x1c[_0xbcec[64]]){_0xe943x16(_0xbcec[65],_0xbcec[66]);_0xe943x11();return false}else {_0xe943x14();return true}},_0xe943x14=function(){var _0xe943x19=_0xe943x3[_0xbcec[18]](_0xe943x7[_0xbcec[17]])[_0xbcec[16]]();if(!_0xe943x19[_0xbcec[29]]){return};if(_0xe943x7[_0xbcec[67]]&& (_0xe943x7[_0xbcec[68]]=== false|| _0xe943x7[_0xbcec[68]]> 0)){_0xe943x10(_0xe943x19);if(_0xe943xb[_0xbcec[56]]()<= _0xe943xa[_0xbcec[56]]()){_0xe943x12()};_0xe943xc[_0xbcec[49]](_0xbcec[48])[_0xbcec[70]](_0xbcec[69],function(){return _0xe943x12()});if(_0xe943x7[_0xbcec[68]]> 0){_0xe943x7[_0xbcec[68]]--}}else {_0xe943xc[_0xbcec[49]](_0xbcec[48]);_0xe943x19[_0xbcec[70]](_0xbcec[71],function(){_0xe943x10(_0xe943x19);_0xe943x15();return false})}},_0xe943x15=function(){var _0xe943x1b=_0xe943x3[_0xbcec[18]](_0xbcec[50])[_0xbcec[16]](),_0xe943x1c=_0xe943x3[_0xbcec[9]](_0xbcec[1]);_0xe943x1c[_0xbcec[58]]= true;_0xe943x1b[_0xbcec[77]](_0xbcec[76])[_0xbcec[45]](_0xbcec[46])[_0xbcec[75]]()[_0xbcec[74]](_0xbcec[72]+ _0xe943x7[_0xbcec[28]]+ _0xbcec[73]);return _0xe943x3[_0xbcec[86]]({scrollTop:_0xe943x1b[_0xbcec[60]]()},0,function(){_0xe943x1b[_0xbcec[18]](_0xbcec[85])[_0xbcec[75]]()[_0xbcec[84]](_0xe943x1c[_0xbcec[64]],function(_0xe943x23,_0xe943x24){if(_0xe943x24=== _0xbcec[78]){return _0xe943x11()};var _0xe943x19=_0xe943x1(this)[_0xbcec[18]](_0xe943x7[_0xbcec[17]])[_0xbcec[16]]();_0xe943x1c[_0xbcec[58]]= false;_0xe943x1c[_0xbcec[64]]= _0xe943x19[_0xbcec[21]](_0xbcec[20])?_0xe943x1[_0xbcec[24]](_0xe943x19[_0xbcec[21]](_0xbcec[20])+ _0xbcec[22]+ _0xe943x7[_0xbcec[23]]):false;_0xe943x1(_0xbcec[80],_0xe943x3)[_0xbcec[79]]();_0xe943x13();if(_0xe943x7[_0xbcec[81]]){_0xe943x7[_0xbcec[81]][_0xbcec[82]](this)};_0xe943x16(_0xbcec[83],_0xe943x1c)})})},_0xe943x16=function(_0xe943x25){if(_0xe943x7[_0xbcec[87]]&&  typeof console=== _0xbcec[88]&& ( typeof _0xe943x25=== _0xbcec[88]||  typeof console[_0xe943x25]=== _0xbcec[10])){if( typeof _0xe943x25=== _0xbcec[88]){var _0xe943x26=[];for(var _0xe943x27 in _0xe943x25){if( typeof console[_0xe943x27]=== _0xbcec[10]){_0xe943x26= (_0xe943x25[_0xe943x27][_0xbcec[29]])?_0xe943x25[_0xe943x27]:[_0xe943x25[_0xe943x27]];console[_0xe943x27][_0xbcec[89]](console,_0xe943x26)}else {console[_0xbcec[90]][_0xbcec[89]](console,_0xe943x26)}}}else {console[_0xe943x25][_0xbcec[89]](console,Array[_0xbcec[92]][_0xbcec[91]][_0xbcec[82]](arguments,1))}}};_0xe943x3[_0xbcec[9]](_0xbcec[1],_0xe943x1[_0xbcec[12]]({},_0xe943x5,{initialized:true,waiting:false,nextHref:_0xe943xd}));_0xe943xf();_0xe943xe();_0xe943x14();_0xe943x1[_0xbcec[12]](_0xe943x3[_0xbcec[1]],{destroy:_0xe943x11});return _0xe943x3};_0xe943x1[_0xbcec[93]][_0xbcec[1]]= function(_0xe943x25){return this[_0xbcec[95]](function(){var _0xe943x28=_0xe943x1(this),_0xe943x1c=_0xe943x28[_0xbcec[9]](_0xbcec[1]),_0xe943x29;if(_0xe943x1c&& _0xe943x1c[_0xbcec[94]]){return};_0xe943x29=  new _0xe943x2(_0xe943x28,_0xe943x25)})}})(jQuery)
```

I created a javascript file that decodes the array at the beggining of the script. I console logged the encoded strings and replaced the array with UTF-8 encoded strings.

I have noticed that there is something related to localStorage so I checked it in the inspector and found the flag.

### **Challenge: Ksteg**
#### Description:
```
This must be a typo.... it was kust one letter away!

Download the file below.
```
We are given an image. Let's extract some info from it:

exiftool results:
```
ExifTool Version Number         : 10.80
File Name                       : luke.jpg
Directory                       : .
File Size                       : 23 kB
File Modification Date/Time     : 2020:06:12 23:44:11+02:00
File Access Date/Time           : 2020:06:12 23:44:12+02:00
File Inode Change Date/Time     : 2020:06:12 23:44:11+02:00
File Permissions                : rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
Image Width                     : 400
Image Height                    : 400
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 400x400
Megapixels                      : 0.160
```

Binwalk results:
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
```

file results:
```
luke.jpg: JPEG image data, baseline, precision 8, 400x400, frames 3
```

There are two typos in the challenge:
- KSteg.
- and kust. (as just)

If you google JSteg, there is a github project "Jsteg - JPEG Steganography", let's install this and extract the data with the tool.

Since I did not have golang installed on my machine and the project is written in it, I had to do that first before trying to extract the data.

```
./jsteg-linux-amd64 reveal luke.jpg
```
Outputs the flag.

## OSINT Category

### **Challenge: Finsta**

#### Description: 
```
This time we have a username. Can you track down NahamConTron?
```

Let's google NahamConTron:
```
Your search - nahamcontron - did not match any documents.
```

Since the challenge is calleg "Finsta" (Fake & instagram), let's see if there is someone called `NahamConTron` on instagram.

The flag is visible on the instagram profile of that user.

### **Challenge: Time Keeper**

#### Description: There is some interesting stuff on this website. Or at least, I thought there was... Connect here: https://apporima.com/ Note, this flag is not in the usual format.

The site navigates us to a blog site. Since this is an OSINT challenge, we will need to research a bit about the person.

- Twitter Page: https://twitter.com/apporima
    - Pinned tweet: https://twitter.com/apporima/status/1202795940576448513
- Name and surname: Trevor Bryant
- From: Washington DC
- Github: https://github.com/trevorbryant

- ##### First blog (desolate - 17.04.19)
    - The blog post consists of an image of R2D2 and C3PO with text `What a desolate place this is.` from Star Wars in ASCII format that is also an image. The image has a title attribute saying: `Don???t call me a mindless philosopher, you overweight glob of grease!`
        - The image is named `churchmag.png`
        - The image does not contain any crucial information in it's metadata.

In the source of the page, we have comments in almost every blog post.
- ##### BUZZ WORDS
    - Located in a p tag in the article tag `DevOps: A History in Configuration Management`
- ##### I fear that they may have just hit the 'move fast and break things' era that industry is finally realizing was a horrible idea. -- @peiriannydd
    - Located in a p tag in the article tag `Rookie of the Year`

Web.archive:
- snapshot 09.05.20
    - The only change is that the BUZZ WORDS comment is not there anymore.
- snapshot 18.04.20
    - There is a new blog that has the following description: `Today, I created my first CTF challenge. The flag can be found at forward slash flag dot txt`
    - If we append /flag.txt to the current web.archive URL, we get the flag.