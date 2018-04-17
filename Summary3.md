{\rtf1\ansi\ansicpg1252\cocoartf1561\cocoasubrtf400
{\fonttbl\f0\fswiss\fcharset0 Helvetica;}
{\colortbl;\red255\green255\blue255;}
{\*\expandedcolortbl;;}
\margl1440\margr1440\vieww15660\viewh13480\viewkind0
\pard\tx720\tx1440\tx2160\tx2880\tx3600\tx4320\tx5040\tx5760\tx6480\tx7200\tx7920\tx8640\pardirnatural\partightenfactor0

\f0\fs24 \cf0 # Oops/Sl@ckers\
## Week13, Documentation of First Results\
### Pavel Danek\
\
Greetings to the whole team, I **finally** made some progress and breakthrough on the way to a **customized bootable iPXE USB drive**.\
I studied [iPXE website](https://ipxe.org/start), [iPXE source code](https://github.com/ipxe/ipxe) and also [NetBoot.xyz website](https://netboot.xyz) and [NetBoot.xyz source code](https://github.com/antonym/netboot.xyz) to better understand, how their scripts work. Scripts from NetBoot.xyz served me as examples and templates to my own. My Linux class teacher, [Mr. Matthew Harmon](https://github.com/mjhedu) was a big help and advisor to me. I also searched advice from NetBoot.xyz\'92s programmer, Antony Messerli and iPXE\'92s no.1, Michael Brown.\
\
1. First guideline came from iPXE website, saying: \'93clone the iPXE repo; cd ipxe/src; make\'94. While that was a good start, it would only build a generic iPXE. An important part are packages one needs to install before building iPXE. They\'92re listed [here](https://ipxe.org/download). With liblzma I had a problem, couldn\'92t find it, but I found a solution in the end: \'93sudo apt-get install -y liblzma-dev\'94.\
2. For customization, I created custom script, called pavelboot and used \'93make EMBED=/home/pavel/ipxe/pavelboot\'94. Keep it in ipxe/ directory. I omitted another argument, seen at NetBoot.xyz, TRUST=XXXXX.crt,YYYYY.crt, which were custom root certificates, that weren\'92t available and only were causing an error.\
3. In order to use https and port 443, which our server communicates through, I had to \'93turn on\'94 https, which is by default off in iPXE:\
	In ipxe/src/config dir:\
	- nano general.h\
	- on line 57, change #undef DOWNLOAD_PROTO_HTTPS to #define DOWNLOAD_PROTO_HTTPS, save and exit\
4. Next obstacle was an error with missing boot image \'91isolines.bin\'92. The solution was found for me as follows: \
	In your home directory:\
	- wget https://mirrors.edge.kernel.org/pub/linux/utils/boot/syslinux/syslinux-6.03.zip\
	- mkdir syslinux-6.03\
	- cd syslinux-6.03\
	- unzip ../syslinux-6.03.zip\
	In ipxe/src directory (assuming it\'92s in your home dir) (in my case, add the embedded file):\
	- make ISOLINUX_BIN=/home/<your home dir>/syslinux-6.03/bios/core/isolinux.bin LDLINUX_C32=/home/<your home dir>/syslinux-6.03/bios/com32/elflink/ldlinux/ldlinux.c32 EMBED=/<path to your embedded file>/<filename>\
5. Then I went to ipxe/src/bin and copied \'93ipxe.iso\'94.\
6. I brought it to Windows environment and using USB burning software Rufus and ipxe.iso file, I created a bootable USB\
7. When booting from it, iPXE environment calls my custom script, [pavelboot](Scripts/pavelboot), which finds our server and starts [pboot.ipxe](Scripts/pboot.ipxe), which then calls the Windows bootloader (its path in the variable TARGET, for now pointing to temporary script called [booter](Scripts/booter), that has no other function but to demonstrate the execution of a real boot loading file.\
8. The final result looks something like this:\
[Picture #1, booting customized iPXE from USB](Pix/IMG_1758.JPG)\
\
Now we have to figure out the \'93Windows part\'94, or how to get a bootable Windows 7 on our server!\
Good luck to us all!\
I\'92ll see you in class!\
\
- Pavel.\
\
}