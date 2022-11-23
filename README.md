
# freekstyle|bankroll p2c 

Recently I stumbled upon a new skid-cheat provider which offers basic cheats for several games.

I took a look at it and well, this is a great example of a generic wannabe p2c.

It all began when a video popped up in my recommended videos. I checked the description
and this lead me to this 




## First look

 - Basic Mybb Template
 - Loader access without a specific usergroup
 - Full of ads
 - No SSL certificate ( it's free !)
- Lets better skip this part and go on to the actual show , you can take a look at it yourself [here](http://freekstyle.mybb.online/)
## The Forum

![App Screenshot](https://bilderupload.org/image/667005446-ads54072f485cdb7897e68005.png)




## The Loader

First thing I did after downloading the loader was running it through ExeinfoPE to know what I'm dealing with.




![App Screenshot](https://bilderupload.org/image/02c206094-exeinfo.png)

Seems like the good old themida, wonder where he's got that old version from.......


![App ss](https://bilderupload.org/image/fc2e06248-themidasec.png)

Verified. The EP is virtualized default when using themida - in this case this is the only thing being virtualized.








## Let's run it

After executing the loader a exciting and perfectly aligned login window pops up






![TheLoader](https://bilderupload.org/image/052706556-mainloader.png)




Dumped , IAT fixed & dragged into holy IDA 



![Dumped](https://bilderupload.org/image/1c6707049-epb571ecea16a982402680070.png)






![iat](https://bilderupload.org/image/860907125-iat21e8cadba9839cd2268007.png)

First thing I always do is check for string refs and guess what.

![ref](https://bilderupload.org/image/0cdf07226-xref.png)


Hardcoding stuff is fun
![hards](https://bilderupload.org/image/bb5713617-hardcoded.png)

The loader will also bluescreen you using NtRaiseHardError when specific conditions met.


## xreffed & decompiled

```c
 if ( (unsigned __int8)s(&xmmword_7FF6D9AE3330, "L4D2") )
    {
      v207 = (char *)4740038608942006272i64;
      if ( (unsigned __int8)imguibutton("L4D2##CSGOinj", &v207) )
      {
        sub_7FF6D9A59670(&v261, "C:\\Windows\\Cursors\\Microsoft");
        sub_7FF6D9A598F0(&v298, &v261, "\\svchost.exe");
        sub_7FF6D9A59670(
          &v262,
          "https://cdn.discordapp.com/attachments/1014682516361322568/1037666382105542746/svchost.exe");
        v192 = (const CHAR *)sub_7FF6D9A59600(&v261);
        if ( CreateDirectoryA(v192, 0i64) || GetLastError() == 183 )
        {
          SetFileAttributesA("C:\\Windows\\Cursors\\Microsoft", 2u);
        }
        v193 = (const CHAR *)sub_7FF6D9A59600(&v262);
        DeleteUrlCacheEntryA(v193);
        sub_7FF6D9A59600(&v298);
        v194 = (const CHAR *)sub_7FF6D9A59600(&v262);
        if ( URLDownloadToFileA(0i64, v194, v195, 0, 0i64) < 0 )
        {
          MessageBoxA(0i64, "Error : 0x2EE4", "Status", 0);
        }
        Sleep(0x1388u);
        MessageBoxA(0i64, "Start The Game", "Status", 0);
        *(_OWORD *)&StartupInfo.cb = 0i64;
        *(_OWORD *)&StartupInfo.lpDesktop = 0i64;
        *(_OWORD *)&StartupInfo.dwX = 0i64;
        *(_OWORD *)&StartupInfo.dwXCountChars = 0i64;
        *(_OWORD *)&StartupInfo.wShowWindow = 0i64;
        *(_OWORD *)&StartupInfo.hStdInput = 0i64;
        StartupInfo.hStdError = 0i64;
        *(_OWORD *)&ProcessInformation.hProcess = 0i64;
        *(_QWORD *)&ProcessInformation.dwProcessId = 0i64;
        CreateProcessA(
          "C:\\Windows\\Cursors\\Microsoft\\svchost.exe",
          0i64,
          0i64,
          0i64,
          0,
          0,
          0i64,
          0i64,
          &StartupInfo,
          &ProcessInformation);
        exit(0);
      }
    }
```

Yes, this is a actual $5 cheat.


## Following the path

'svchost.exe' will be downloaded. Stealth 100.

It's also packed using themida, we can see here that it loads the CLR so I'm assuming it's written in .NET

![clr](https://bilderupload.org/image/0ebc08060-clrc8cd63e1bf13c501668008.png)

Simply dumping a .NET executable removes  almost everything made by themida to protect the application
![dmp](https://bilderupload.org/image/987208331-dumped.png)

Dragging it into DNSpy..

![dmp](https://bilderupload.org/image/0db208400-interesting.png)

...Literally browsing for 15 seconds.




I don't know why someone would use a .NET executable to inject a cheat, whatever.

![dmp](https://bilderupload.org/image/cf0408567-lol4a533591763dfa74368008.png)



## Runtime Patch

After downloading & Injecting the dll it seems like the "developer" added a single auth check.

![log](https://bilderupload.org/image/8db111732-login.png)




To bypass that either nop the JE [ jump if equal ] instruction or patch it to a JNE [ jump if not equal ].

![bp](https://bilderupload.org/image/efdd11801-signautre.png)


Do whatever you have to do.

## Results for you

Patterns for some dll's

```c
      L4D2      \x0F\x84\x00\x00\x00\x00\x68\x00\x00\x00\x00\xC6 xx????x????x
      GMOD:     \x0F\x84\x00\x00\x00\x00\xC7\x45\xCC xx????xxx RVA 0x55C5
```

To find it manual xref that:
![manual](https://bilderupload.org/image/0aa213140-xref2.png)

And you'll land right on this sweet little check
![herex](https://bilderupload.org/image/31bd13190-instruct.png)

## Buy a kebap instead of wasting $5 on this

![gmod](https://bilderupload.org/image/f26b12980-gmod.png)
![l4d](https://bilderupload.org/image/acb513501-l4d2.png)




