WINDOWS UNITY3D LZMA SPEEDUP
=============================
>   fork这个项目，是因为我第一份游戏开发工作用的u3d开发页游，在实际环境中运行的时候总是莫名崩溃，说是申请8MB内存失败。
    其实就是因为打包后的字典需求内存空间8MB，而我们项目的动态资源包非常多，所以就崩溃了。。。
    怕作者删掉项目，而我暂时用不上，因此fork

How to use
----------
1. Rename your C:\Program Files (x86)\Unity\Editor\Data\Tools\lzma.exe to lzma_real.exe. You can also make a separate backup to a safe place if you wish.
2. Copy lzma.exe from the "windows binaries" directory (or your custom build from sources) into your Unity3D tools folder


>            System.Console.WriteLine("\nUsage:  LZMA <e|d> [<switches>...] inputFile outputFile\n" +
            	"  e: encode file\n" +
            	"  d: decode file\n" +
            	"  b: Benchmark\n" +
            	"<Switches>\n" +
            	// "  -a{N}:  set compression mode - [0, 1], default: 1 (max)\n" +
            	"  -d{N}:  set dictionary - [0, 29], default: 23 (8MB)\n" +
            	"  -fb{N}: set number of fast bytes - [5, 273], default: 128\n" +
            	"  -lc{N}: set number of literal context bits - [0, 8], default: 3\n" +
            	"  -lp{N}: set number of literal pos bits - [0, 4], default: 0\n" +
            	"  -pb{N}: set number of pos bits - [0, 4], default: 2\n" +
            	"  -mf{MF_ID}: set Match Finder: [bt2, bt4], default: bt4\n" +
            	"  -eos:   write End Of Stream marker\n"
            	// + "  -si:    read data from stdin\n"
            	// + "  -so:    write data to stdout\n"
            	);

http://stackoverflow.com/questions/3057171/lzma-compression-settings-details

A log file will be written to lzma_call_cli_arguments_log.txt within the Tools directory where you can have a look at the command line parameters which Unity3D used to call lzma.exe. You can also find the elapsed time of the compression there.

How does it work?
-----------------
Whenever Unity3D uses LZMA compression, the "fake" lzma.exe from this Speedup Project will substitute Unity3Ds max-compression parameters (for e.g. -fb372) to those which provide fastest compression (-a0 -d0 -mt4 -fb5 -mc0 -lc0 -pb0 -mfbt2).

This interceptor can speed up compressing the build by a factor of 4!

---> Important: Speed don't come for free: Compression rate will be worst, only use it in your local development environment! <---

Unity3D Versions
----------------------------
Tested with Unity 3.4 and Unity 3.5 on Windows

News
----
Follow me on Twitter @[derFunk]! Let me know if this little project helped you saving time, and if you experienced even better speedup factors.

[derFunk]: http://twitter.com/derFunk
