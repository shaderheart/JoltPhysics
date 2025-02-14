# Building and Using Jolt Physics

## Build Types

Each platform supports multiple build targets

- Debug - Debug version of the library, turns on asserts
- Release - Release version of the library, no asserts but includes profiling support and can draw the world and simulation properties
- ReleaseASAN - As Release but turns on Address Sanitizer (clang only) to find bugs
- ReleaseUBSAN - As Release but turns on Undefined Behavior Sanitizer (clang only) to find bugs
- ReleaseCoverage - As Release but turns on Coverage reporting (clang only) to find which areas of the code are not executed
- Distribution - Shippable version of the library, turns off all debugging support

## Includes

The Jolt headers don't include Jolt/Jolt.h. Always include Jolt/Jolt.h before including any other Jolt header.
You can use Jolt/Jolt.h in your precompiled header to speed up compilation.

## Defines

There are a number of user configurable defines that turn on/off certain features:

- JPH_PROFILE_ENABLED - Turns on the internal profiler.
- JPH_EXTERNAL_PROFILE - Turns on the internal profiler but forwards the information to a user defined external system (see Profiler.h).
- JPH_DEBUG_RENDERER - Adds support to draw lines and triangles, used to be able to debug draw the state of the world.
- JPH_DISABLE_TEMP_ALLOCATOR - Disables the temporary memory allocator, used mainly to allow ASAN to do its job.
- JPH_DISABLE_CUSTOM_ALLOCATOR - Disables the ability to override the memory allocator.
- JPH_FLOATING_POINT_EXCEPTIONS_ENABLED - Turns on division by zero and invalid floating point exception support in order to detect bugs (Windows only).
- JPH_CROSS_PLATFORM_DETERMINISTIC - Turns on behavior to attempt cross platform determinism. If this is set, JPH_USE_FMADD is ignored.
- JPH_DOUBLE_PRECISION - Compiles the library so that all positions are stored in doubles instead of floats. This makes larger worlds possible.
- JPH_USE_SSE4_1 - Enable SSE4.1 CPU instructions (x86/x64 only)
- JPH_USE_SSE4_2 - Enable SSE4.2 CPU instructions (x86/x64 only)
- JPH_USE_F16C - Enable half float CPU instructions (x86/x64 only)
- JPH_USE_LZCNT - Enable the lzcnt CPU instruction (x86/x64 only)
- JPH_USE_TZCNT - Enable the tzcnt CPU instruction (x86/x64 only)
- JPH_USE_AVX - Enable AVX CPU instructions (x86/x64 only)
- JPH_USE_AVX2 - Enable AVX2 CPU instructions (x86/x64 only)
- JPH_USE_AVX512 - Enable AVX512F+AVX512VL CPU instructions (x86/x64 only)
- JPH_USE_FMADD - Enable fused multiply add CPU instructions (x86/x64 only)

## Logging & Asserting

To override the default trace and assert mechanism install your own custom handlers in Trace and AssertFailed (see IssueReporting.h).

## Custom Memory Allocator

To implement your custom memory allocator override Allocate, Free, AlignedAllocate and AlignedFree (see Memory.h).

## Building

<details>
	<summary>Windows 10+</summary>
	<ul style="list-style: none"><li>
		<details>
			<summary>MSVC CL (default compiler)</summary>
			<ul>
				<li>Download Visual Studio 2022 (Community or other edition)</li>
				<li>Download CMake 3.15+ (https://cmake.org/download/)</li>
				<li>Run cmake_vs2022_cl.bat</li>
				<li>Open the resulting project file VS2022_CL\JoltPhysics.sln</li>
				<li>Compile and run either 'Samples' or 'UnitTests'</li>
			</ul>
		</details>
		<details>
			<summary>MSVC CL - 32 bit</summary>
			<ul>
				<li>Download Visual Studio 2022 (Community or other edition)</li>
				<li>Download CMake 3.15+ (https://cmake.org/download/)</li>
				<li>Run cmake_vs2022_cl_32bit.bat</li>
				<li>Open the resulting project file VS2022_CL_32BIT\JoltPhysics.sln</li>
				<li>Compile and run either 'Samples' or 'UnitTests'</li>
			</ul>
		</details>
		<details>
			<summary>MSVC Clang compiler</summary>
			<ul>
				<li>Download Visual Studio 2022 (Community or other edition)</li>
				<li>Make sure to install "C++ Clang Compiler for Windows 11.0.0+" and "C++ Clang-cl for v142+ build tools (x64/x86)" using the Visual Studio Installer</li>
				<li>Download CMake 3.15+ (https://cmake.org/download/)</li>
				<li>Run cmake_vs2022_clang.bat</li>
				<li>Open the resulting project file VS2022_Clang\JoltPhysics.sln</li>
				<li>Compile and run either 'Samples' or 'UnitTests'</li>
			</ul>
		</details>
		<details>
			<summary>MSVC Universal Windows Platform</summary>
			<ul>
				<li>Download Visual Studio 2022+ (Community or other edition)</li>
				<li>Make sure to install "Universal Windows Platform development" using the Visual Studio Installer</li>
				<li>Download CMake 3.15+ (https://cmake.org/download/)</li>
				<li>Run cmake_vs2022_uwp.bat</li>
				<li>Open the resulting project file VS2022_UWP\JoltPhysics.sln</li>
				<li>Compile and run 'UnitTests'</li>
			</ul>
		</details>
		<details>
			<summary>MinGW</summary>
			<ul>
				<li>Follow download instructions for MSYS2 (https://www.msys2.org/)</li>
				<li>From the MSYS2 MSYS app run: pacman -S --needed mingw-w64-x86_64-toolchain mingw-w64-x86_64-cmake</li>
				<li>From the MSYS2 MINGW x64 app, in the Build folder run: ./cmake_windows_mingw.sh</li>
				<li>Run: cmake --build MinGW_Debug</li>
				<li>Run: MinGW_Debug/UnitTests.exe</li>
			</ul>
		</details>
	</li></ul>
</details>

<details>
	<summary>Linux</summary>
	<ul style="list-style: none"><li>
		<details>
			<summary>Debian flavor, x64 or ARM64</summary>
			<ul>
				<li>Install clang (apt-get install clang)</li>
				<li>Install cmake (apt-get install cmake)</li>
				<li>Run: ./cmake_linux_clang_gcc.sh</li>
				<li>Go to the Linux_Debug folder</li>
				<li>Run: make -j$(nproc) && ./UnitTests</li>
			</ul>
		</details>
		<details>
			<summary>Debian flavor, MinGW Cross Compile</summary>
			<ul>
				<li>This setup can be used to run samples on Linux using wine and vkd3d. Tested on Ubuntu 22.04</li>
				<li>Graphics card must support Vulkan and related drivers must be installed</li>
				<li>Install mingw-w64 (apt-get install mingw-w64)</li>
				<li>Run: update-alternatives --config x86_64-w64-mingw32-g++ (Select /usr/bin/x86_64-w64-mingw32-g++-posix)</li>
				<li>Install cmake (apt-get install cmake)</li>
				<li>Install wine64 (apt-get install wine64)</li>
				<li>Run: export WINEPATH="/usr/x86_64-w64-mingw32/lib;/usr/lib/gcc/x86_64-w64-mingw32/10-posix" (change it based on your environment)</li>
				<li>Run: ./cmake_linux_mingw.sh Release (Debug doesn't work)</li>
				<li>Go to the MinGW_Release folder</li>
				<li>Run: make -j$(nproc) && wine UnitTests.exe</li>
				<li>Run: wine Samples.exe</li>
			</ul>
		</details>
	</li></ul>
</details>

<details>
	<summary>Android</summary>
	<ul>
		<li>Install Android Studio 2020.3.1+ (https://developer.android.com/studio/)</li>
		<li>Open the 'Android' folder in Android Studio and wait until gradle finishes</li>
		<li>Select 'Run' / 'Run...' and 'UnitTests'</li>
		<li>If the screen turns green after a while the unit tests succeeded, when red they failed (see the android log for details)</li>
	</ul>
</details>

<details>
	<summary>macOS</summary>
	<ul>
		<li>Install XCode</li>
		<li>Download CMake 3.23+ (https://cmake.org/download/)</li>
		<li>Run: ./cmake_xcode_macos.sh</li>
		<li>This will open XCode with a newly generated project</li>
		<li>Build and run the project</li>
		<li>Note that you can also follow the steps in the 'Linux' section if you wish to build without XCode.</li>
	</ul>
</details>

<details>
	<summary>iOS</summary>
	<ul>
		<li>Install XCode</li>
		<li>Download CMake 3.23+ (https://cmake.org/download/)</li>
		<li>Run: ./cmake_xcode.ios.sh</li>
		<li>This will open XCode with a newly generated project</li>
		<li>Build and run the project (note that this will only work in the simulator as the code signing information is not set up)</li>
	</ul>
</details>



## Other Build Tools

* A vcpkg package is available [here](https://github.com/microsoft/vcpkg/tree/master/ports/joltphysics).
* A xmake package is available [here](https://github.com/xmake-io/xmake-repo/tree/dev/packages/j/joltphysics).
* Jolt has been verified to build with [ninja](https://ninja-build.org/) through CMake.

## Link Errors

If you receive the following error when linking:

```
/usr/bin/ld: libJolt.a: error adding symbols: file format not recognized
```

Then you have not enabled interprocedural optimizations (link time optimizations) for your own application. See the INTERPROCEDURAL_OPTIMIZATION option in CMakeLists.txt.

## Doxygen on Windows

Documentation can be generated through doxygen:

- Install Doxygen (https://www.doxygen.nl/download.html)
- Run: run_doxygen.bat
