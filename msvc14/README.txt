This directory includes hand-crafted project files for building SoX under
MSVC15. The project files may be replaced by expanding CMAKE support in the
future, but for now, this is the easiest way to build SoX with MS Visual C++.
The resulting sox.exe has support for all SoX features except magic, and
pulseaudio. LAME (libmp3lame.dll or lame_enc.dll), MAD (libmad.dll or
cygmad-0.dll), libsndfile (libsndfile-1.dll) and AMR support (libamrnb-3.dll,
libamrwb-3.dll) are loaded at runtime if they are available.

How to build:

1. Check out the SoX git code into a directory named sox.

2. Extract the source code for the other libraries next to the sox
   directory. Remove the version numbers from the directory names.
   
	The following versions were tested and successfully built:
   
   -- flac-1.3.2.tar.gz (http://downloads.xiph.org/releases/flac/flac-1.3.2.tar.xz) extracted into directory flac 
   -- lame-399.5.tar.gz (https://sourceforge.net/projects/lame/files/lame/3.99/lame-3.99.5.tar.gz/download) extracted into directory lame
   -- libid3tag-0.15.1b.tar.gz (https://sourceforge.net/projects/mad/files/libid3tag/0.15.1b/libid3tag-0.15.1b.tar.gz/download) extracted into directory libid3tag
   -- libmad-0.15.1b.tar.gz (https://sourceforge.net/projects/mad/files/libmad/0.15.1b/libmad-0.15.1b.tar.gz/download) extracted into directory libmad
   -- libogg-1.3.2.tar.gz (http://downloads.xiph.org/releases/ogg/libogg-1.3.2.tar.xz) extracted into directory libogg
   -- libpng-1.6.28.tar.gz (http://prdownloads.sourceforge.net/libpng/libpng-1.6.28.tar.gz?download) extracted into directory libpng
   -- libvorbis-1.3.5.tar.gz (http://downloads.xiph.org/releases/vorbis/libvorbis-1.3.5.tar.xz) extracted into directory libvorbis
   -- speex-1.2rc1.tar.gz (http://downloads.xiph.org/releases/speex/speex-1.2rc1.tar.gz)extracted into directory speex
   -- speexDsp-1.2rc3.tar.gz (https://github.com/xiph/speexdsp/archive/SpeexDSP-1.2rc3.tar.gz)extracted into directory speex

   -- wavpack-5.1.0.tar.bz2 (https://github.com/dbry/WavPack/releases/tag/5.1.0) extracted into directory wavpack
   -- zlib-1.2.11.tar.gz (http://www.zlib.net/zlib-1.2.11.tar.gz) extracted into directory zlib

   -- libsndfile-1.0.26.tar.gz ( http://www.mega-nerd.com/libsndfile/files/libsndfile-1.0.26.tar.gz) extracted into directory libsndfile 

   
3. Open the sox\msvc14\SoX.sln solution.

4. If any of the above libraries are not available or not wanted, adjust the
   corresponding settings in the soxconfig.h file (in the LibSoX project inside
   the Config Files folder) and remove the corresponding project from the
   solution.

5. Build the solution.

6. The resulting executable files will be in sox\msvc14\Debug or
   sox\msvc14\Release. The resulting sox.exe will dynamically link to
   libmp3lame.dll, libmad.dll, libsndfile-1.dll, libamrnb-3.dll, and
   libamrwb-3.dll if they are available, but will run without them (though the
   corresponding features will be unavailable if they are not present).

Points to note:

- The libsndfile-1.0.20.tar.gz package does not include the sndfile.h header
  file. Normally, before compiling libsndfile, you would create sndfile.h
  (either by processing it via autoconf, by downloading a copy, or by renaming
  sndfile.h.in). However, this SoX solution includes its own version of
  sndfile.h, so you should not create a sndfile.h under the libsndfile folder.
  To repeat: you should extract a clean copy of libsndfile-1.0.20.tar.gz, and
  should not add, process, or rename any files.

- The solution includes an experimental effect called speexdsp that uses the
  speex DSP library. This does not yet enable any support for the speex file
  format or speex codec. The speexdsp effect is simply an experimental effect
  to make use of the automatic gain control and noise filtering components that
  are part of the speex codec package. Support for the speex codec may be added
  later.

- The included projects do not enable SSE2. You can enable this in the project
  properties under Configuration Properties, C/C++, Code Generation, Enable
  Enhanced Instruction Set. Note that some editions of Visual Studio might
  not include Enhanced Instruction Set support.

- The included projects set the floating-point model to "fast". This means
  that the compiler is free to optimize floating-point operations. For
  example, the compiler might optimize the expression (14.0 * x / 7.0) into
  (x * 2.0). In addition, the compiler is allowed to leave expression results
  in floating-point registers to store temporary values instead of rounding
  each intermediate result to a 32-bit or 64-bit value. In some cases, these
  optimizations can change the results of floating-point calculations. If you
  need more precise results, you can change this optimization setting can be
  changed to one of the other values. The "precise" setting avoids any
  optimization that might change the result (preserves the order of all
  operations) but keeps optimizations that might give more accurate results
  (such as using more precision than necessary for intermediate values if doing
  so results in faster code). The "strict" setting avoids any optimization that
  might change the result in any way contrary to the C/C++ standard and rounds
  every intermediate result to the requested precision according to standard
  floating-point rounding rules. You can change this setting in the project
  properties under Configuration Properties, C/C++, Code Generation, Floating
  Point Model.
