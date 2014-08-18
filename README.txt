
README PJSIP CSHARP

This is a work in progress.

My goal is to create C# wrappers for PJSIP such that it can be used on Windows as well as Xamarin - Android/iOS.

The first step is to produce a stand along PJSIP windows dll. For that I used Mike G's comments as a starting point.

   https://github.com/siniypin/pjsip4net/issues/15

Here is my updated set of instructions for getting PJSIP to compile on Windows. 

1) Create a new project. (I named it PJSUA2, and it is checked in.)
2) Install SWIG. (I used version 3.0) and had to add it to the system environment variables so I could execute it in the cmd window.

   http://www.swig.org/

3) Create a csharp directory off of the pjsip-apps\src\swig directory.
4) In that directory execute:

```
swig -I../../../../pjlib/include -I../../../../pjlib-util/include -I../../../../pjmedia/include -I../../../../pjsip/include -I../../../../pjnath/include -w312 -c++ -csharp -o pjsua2_wrap.cpp ../pjsua2.i
```

	The -c++ is important otherwise it produces c code which VS has problems with.
	
5) Create a new Win32 DLL project. Copy the produced .h and .cpp files into the new project. 
Then I updated the output directory, include files, lib search path and libs to: (In PJSUA2\properties [select ALL Configurations)

Output: (General -> Output Directory)

    .\output\$(ProjectName)-$(TargetCPU)-$(PlatformName)-vc$(VSVer)-$(Configuration)\

C/C++ (C/C++\General -> Additional Include Directories)

    ..\pjsip\include;..\pjlib\include;..\pjlib-util\include;..\pjmedia\include;..\pjnath\include;%(AdditionalIncludeDirectories)

Additional Library Directories: (Linker\All Options -> Additional Library Directories)

    ..\pjsip\lib;..\pjlib-util\lib;..\pjlib\lib;..\pjnath\lib;..\pjmedia\lib;..\third_party\lib;%(AdditionalLibraryDirectories)
	
Then for Debug only so far, added these additional dependencies directly:

	pjsua-lib-i386-Win32-vc8-Debug-Dynamic.lib;
	pjsua2-lib-i386-Win32-vc8-Debug-Dynamic.lib;
	pjlib-i386-Win32-vc8-Debug-Dynamic.lib;
	pjlib-util-i386-Win32-vc8-Debug-Dynamic.lib;
	pjsip-core-i386-Win32-vc8-Debug-Dynamic.lib;
	pjnath-i386-Win32-vc8-Debug-Dynamic.lib;
	pjmedia-i386-Win32-vc8-Debug-Dynamic.lib;
	pjmedia-audiodev-i386-Win32-vc8-Debug-Dynamic.lib;
	pjsip-simple-i386-Win32-vc8-Debug-Dynamic.lib;
	pjsip-ua-i386-Win32-vc8-Debug-Dynamic.lib;
	libspeex-i386-Win32-vc8-Debug-Dynamic.lib;
	pjmedia-codec-i386-Win32-vc8-Debug-Dynamic.lib;
	libsrtp-i386-Win32-vc8-Debug-Dynamic.lib;
	libgsmcodec-i386-Win32-vc8-Debug-Dynamic.lib;
	libresample-i386-Win32-vc8-Debug-Dynamic.lib;
	libilbccodec-i386-Win32-vc8-Debug-Dynamic.lib;
	Ws2_32.lib;

I added those names to the debug configuration since I couldn't get the VC macros to match up exactly. I think it can be done, but would just take some time.

Then I set the dependencies on the projects for the above lib files and set the active configuration to Debug-Dynamic so it would pull in the right VC libs.

6) Created a new C# library project. I called it pjsua2_net and added all the cs files created from step 4.
7) Set pjsua2 as its dependency.

That is as far as I've gotten at this point. I'm looking for help to verify the steps above and continue.

All my projects & work are checked for how far I've gotten. That is, all the above steps applied to PJSIP.