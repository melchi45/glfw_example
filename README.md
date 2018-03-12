# glfw_example
glfw example

### Initialize submodule
```
git submodule init
git submodule update
```

## install Microsoft vcpkg

about vcpkg from this url.
 - https://blogs.msdn.microsoft.com/vcblog/2016/09/19/vcpkg-a-tool-to-acquire-and-build-c-open-source-libraries-on-windows/
 - https://docs.microsoft.com/en-us/cpp/vcpkg

if your install vcpkg to D:\Tools folder
```
d:
mkdir Tools
cd D:\Tools\
```

Clone vcpkg repository, then run
```
git clone https://github.com/Microsoft/vcpkg
cd vcpkg
.\bootstrap-vcpkg.bat
```

Then, to hook up user-wide integration, run (note: requires admin on first use)
```
.\vcpkg integrate install
PS D:\Tools\vcpkg> .\vcpkg integrate install
Applied user-wide integration for this vcpkg root.

All MSBuild C++ projects can now #include any installed libraries.
Linking will be handled automatically.
Installing new libraries will make them instantly available.

CMake projects should use: "-DCMAKE_TOOLCHAIN_FILE=D:/Tools/vcpkg/scripts/buildsystems/vcpkg.cmake"
```

Install any packages for x86(x86-windows), x64(x64-windows), arm(arm-windows), arm64(arm64-winodws) windows with
```
.\vcpkg install ffmpeg:x64-windows ffmpeg:x86-windows
.\vcpkg install sdl2:x64-windows sdl2:x86-windows
.\vcpkg install pthreads:x64-windows pthreads:x86-windows
.\vcpkg install glew:x86-windows glew:x64-windows
.\vcpkg install glfw3:x86-windows glfw3:x64-windows
.\vcpkg install zlib:x86-windows zlib:x64-windows
```

Finally, create a New Project (or open an existing one) in Visual Studio 2017 or 2015. All installed libraries are immediately ready to be `#include`'d and used in your project.
For CMake projects, simply include our toolchain file. See our [using a package](docs/examples/using-sqlite.md) example for the specifics.
## Tab-Completion / Auto-Completion
`Vcpkg` supports auto-completion of commands, package names, options etc. To enable tab-completion in Powershell, use
```
.\vcpkg integrate powershell
```
and restart Powershell.

Check packages list with
```
.\vcpkg list
```

## generate build project using cmake
cmake .. -G "Visual Studio 15 2017" -DVCPKG_TARGET_TRIPLET=x86-windows -DCMAKE_TOOLCHAIN_FILE="D:/Tools/vcpkg/scripts/buildsystems/vcpkg.cmake"