# Windows Bootstrap Resources

A docker + GHA based infrastructure designed to build and distribute binaries for Spack on Windows that are critical to Spack's function, but cannot be built/bootstrapped on the platform.

Required for Spack function on Windows:

| Package          | Requirement                                        | Build Process                                                                        |
|------------------|----------------------------------------------------|--------------------------------------------------------------------------------------|
| File             | Required for file inspection; used with buildcache | Cross compiled via docker image w/ mingw-w64 cross compiler targeting x86_64 Windows |
| Gpg              |                                                    | Cross compiled via docker image w/ mingw-w64 cross compiler targeting x86_64 Windows |
| Mingw-w64        | Needed to build File + Gpg                         | Compiled via docker image w/ gcc                                                     |
| Compiler Wrapper | Windows Compiler wrapper - C++ project             | Built directly by GHA on a Windows runner                                            |

## Motivation

Multiple packages are critical to Spack's functionality, Windows or otherwise, but simply cannot be built natively on Windows, and Spack does not yet support cross compiling/MSYS/Cygwin/etc.

To that end, we developed the infrastructure in this repo to cross compile, via docker images using the mingw-w64 cross compiler (which is also built from source by this repo), the required tools.

Additionally, due to Spack's build cache binary compatibility limitations, we build the Windows compiler wrapper here as well so as to forgoe the Spack bootstrapping process and download these binaries directly