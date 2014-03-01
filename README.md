WINDOWS UNITY3D LZMA SPEEDUP
=============================
>   fork这个项目，是因为我第一份游戏开发工作用的u3d开发页游，在实际环境中运行的时候总是莫名崩溃，说是申请8MB内存失败。
    其实就是因为打包后的字典需求内存空间8MB，而我们项目的动态资源包非常多，所以就崩溃了。。。
    怕作者删掉项目，而我暂时用不上，因此fork

How to use
----------
1. Rename your C:\Program Files (x86)\Unity\Editor\Data\Tools\lzma.exe to lzma_real.exe. You can also make a separate backup to a safe place if you wish.
2. Copy lzma.exe from the "windows binaries" directory (or your custom build from sources) into your Unity3D tools folder
3. 
LZMA Options:

level

Description: The compression level.
Range: [0;9].
Default: 5.
dictSize

Description: The dictionary size.
Range: [1<<12;1<<27] for 32-bit version or [1<<12;1<<30] for 64-bit version.
Default: 1<<24.
lc

Description: The number of high bits of the previous byte to use as a context for literal encoding.
Range [0;8].
Default: 3
Sometimes lc = 4 gives gain for big files.
lp

Description: The number of low bits of the dictionary position to include in literal_pos_state.
Range: [0;4].
Default: 0.
It is intended for periodical data when period is equal 2^value (where lp=value). For example, for 32-bit (4 bytes) periodical data you can use lp=2. Often it's better to set lc=0, if you change lp switch.
pb

Description: pb is the number of low bits of the dictionary position to include in pos_state.
Range: [0;4].
Default: 2.
It is intended for periodical data when period is equal 2^value (where lp=value).
algo

Description: Sets compression mode.
Options: 0 = fast, 1 = normal.
Default: 1.
fb

Description: Sets the number of fast bytes for the Deflate/Deflate64 encoder.
Range: [5;255].
Default: 128.
Usually, a big number gives a little bit better compression ratio and a slower compression process. A large fast bytes parameter can significantly increase the compression ratio for files which contain long identical sequences of bytes.
btMode

Description: Sets Match Finder for LZMA.
Options: 0 = hashChain mode, 1 = binTree mode.
Default: 1.
Default method is bt4. Algorithms from hc* group don't provide a good compression ratio, but they often work pretty fast in combination with fast mode.
numHashBytes

Description: Number of hash bytes. See mf={MF_ID} section here for details.
Options: 2, 3 or 4.
Default: 4.
mc

Description: Sets number of cycles (passes) for match finder.
Range: [1;1<<30].
Default: 32.
If you specify mc = 0, LZMA will use default value. Usually, a big number gives a little bit better compression ratio and slower compression process. For example, mf=HC4 and mc=10000 can provide almost the same compression ratio as mf=BT4.
writeEndMark

Description: Option for writing or not writing the end mark.
Options: 0 - do not write EOPM, 1 - write EOPM.
Default: 0.
numThreads

Description: Number of threads.
Options: 1 or 2
Default: 2

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
