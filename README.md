Guide to install UT2003

#Install the files
```
mkdir ~/Games/ut2003
...
sudo mount -o loop CDx.iso /mount/cdrom
sudo cp -R /mount/cdrom/* ~/Games/ut2003/
```

#CD Key

Put your cd key in a file
```
echo XXXXX-XXXXX-XXXXX-XXXX >> ut2003/System/cdkey
```

#Decompress .uz2 files

You need to decompress .uz2 files by running this
```
for i in ../**/*.uz2 ; do ./ucc-bin decompress $i ; done
```

#libstdc++.so.5

If you get errors like
```
./ut2003-bin: error while loading shared libraries: libstdc++.so.5: cannot open shared object file: No such file or directory
```
Install libstdc++5 package

##Under 32bits (not tested)
```
sudo apt-get install libstdc++5
```
##Under 64bits
```
sudo apt-get install libstdc++5:i386
```

#Configure Render Device

If you get errors like
```
Can't find file for package 'WinDrv'
```
In **System/UT2003.ini** and **System/User.ini** replace
```
RenderDevice=D3DDrv.D3DRenderDevice
;RenderDevice=Engine.NullRenderDevice
;RenderDevice=OpenGLDrv.OpenGLRenderDevice
```
by
```
;RenderDevice=D3DDrv.D3DRenderDevice
;RenderDevice=Engine.NullRenderDevice
RenderDevice=OpenGLDrv.OpenGLRenderDevice
```
and
```
ViewportManager=WinDrv.WindowsClient
;ViewportManager=SDLDrv.SDLClient
```
by
```
;ViewportManager=WinDrv.WindowsClient
ViewportManager=SDLDrv.SDLClient
```

#Bad or no sound

If you get errors like
```
open /dev/[sound/]dsp: No such file or directory
```
Run the game with **padsp**:
```
padsp ./ut2003-bin
```

#ERROR: ld.so: with padsp

If you are using 64-bit system, you might still get no sound. The startup may show the following error when using padsp:
```
ERROR: ld.so: object '/usr/lib/x86_64-linux-gnu/pulseaudio/libpulsedsp.so' from LD_PRELOAD cannot be preloaded (wrong ELF class: ELFCLASS64): ignored.
```
To solve this, install the 32-bit version if not already installed:
```
sudo apt-get install libpulsedsp:i386
```
Then, copy the padsp script to a local (or global) padsp32 file and edit the lines that set the LD_PRELOAD variable to point to the 32-bit version of the library (use the locate command). Example:
```
...
if [ x"$LD_PRELOAD" eq x ] ; then
  LD_PRELOAD="/usr/lib/i386-linux-gnu/pulseaudio/libpulsedsp.so"
else
  LD_PRELOAD="$LD_PRELOAD /usr/lib/i386-linux-gnu/pulseaudio/libpulsedsp.so"
fi
...
```
Run the game with **padsp32**:
```
padsp32 ./ut2003-bin
```

#Important

ut2003 creates ~/.ut2003 directory
