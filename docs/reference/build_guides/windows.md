# Windows 10/11

### Enable long path support (if you haven't already)

Find instructions here: [Maximum File Path Limitation](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=registry)

### Install git

Get it here: [Git Download](https://git-scm.com/download/win)

### Install MS Visual Studio 2022

Get it here: [Microsoft Visual Studio](https://visualstudio.microsoft.com/vs/)
Ensure CMake tools for Windows is included on install. [CMake projects in Visual Studio](https://learn.microsoft.com/en-us/cpp/build/cmake-projects-in-visual-studio?view=msvc-170#installation)
The "CMake tools" component bundles `cmake` and `ninja` (xSTUDIO's CMake generator) along with the MSVC compiler, so no separate install is needed.
Restart your machine after Visual Studio finishes installing.

### Download and install Qt 6.5.3 SDK

Follow [these instructions](downloading_qt.md)

### Download and install the NSIS tool

NSIS is a packaging system that lets us build xSTUDIO into a Windows installer exe file. Follow the download link on the [NSIS homepage](https://nsis.sourceforge.io/Download). This will download an installer .exe file. Run this program and follow through the steps in the installer wizard with the default installation options until you hit 'Finish'. You can close the NSIS window, it doesn't need to be running for the next steps.

### Download the VCPKG repo

Start a Windows Powershell to continue these instructions, where you must run a handfull of powershell commands to build xSTUDIO. Windows Powershell is pre-installed, to start it type Powershell into the Search bar in the Start menu. You will need a location to build xSTUDIO from. We recommend making a folder in your home space, called something like 'dev', as follows:

    mkdir dev
    cd dev

To build xSTUDIO we need a number of other open source software packages. We use the VCPKG package manager to do this. All that we need to do is download the repo and run the bootstrap script before we build xstudio. Run these commands in the Powershell:

    git clone https://github.com/microsoft/vcpkg.git
    git -C vcpkg checkout c2aeddd80357b17592e59ad965d2adf65a19b22f
    ./vcpkg/bootstrap-vcpkg.bat

### Download the xSTUDIO repo

Download from github in the usual manner. Enter the root folder of the repo and ensure you are building from the correct branch. Example terminal commands might be as follows, to build from the develop branch:

    git clone https://github.com/AcademySoftwareFoundation/xstudio.git
    cd xstudio
    git checkout develop

### Tell CMake where Qt is installed

CMake needs to know where your Qt 6.5.3 SDK is installed. Create a `CMakeUserPresets.json` file alongside `CMakePresets.json` in the repo root. This file is gitignored, so your local path won't be committed. The user preset should have a different name from the tracked preset it inherits from, and add `Qt6_DIR` to `cacheVariables`. For example, if user Mary Jane downloaded Qt into the root of her C: drive:

    {
      "version": 3,
      "configurePresets": [
        {
          "name": "WinNinjaReleaseLocal",
          "inherits": "WinNinjaRelease",
          "cacheVariables": {
            "Qt6_DIR": "C:/Qt/6.5.3/msvc2019_64/lib/cmake/Qt6"
          }
        }
      ]
    }

See the [CMake presets documentation](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html) for the full format reference.

### Set up the build environment

The `cmake` and `ninja` tools, along with the MSVC compiler, are bundled with Visual Studio 2022's "CMake tools" component but are not on your `PATH` by default. Make them available in your PowerShell session by entering the Visual Studio Developer Shell:

    Import-Module "C:\Program Files\Microsoft Visual Studio\2022\Community\Common7\Tools\Microsoft.VisualStudio.DevShell.dll"
    Enter-VsDevShell -VsInstallPath "C:\Program Files\Microsoft Visual Studio\2022\Community" -Arch amd64 -SkipAutomaticLocation

After running those two commands, `cmake` and `ninja` will resolve directly from the command line for the rest of the session, and the build commands below work as shown.

### Build xSTUDIO

Note that the first cmake command below ***may take several hours to complete***. This is because xSTUDIO's dependencies (particularly ffmpeg) take a long time to download and build from the source code, which is what VCPKG is doing.

Configure:

    cmake -B build --preset WinNinjaReleaseLocal

Build:

    cmake --build build --target package

> **Note:** `--target package` produces the portable ZIP (and the NSIS installer, if NSIS is installed) and is significantly slower than a plain build. When iterating during development, drop the `--target package` flag and just run `cmake --build build`.

RelWithDebInfo and Debug variants are also available — see [CMakePresets.json](../../../CMakePresets.json) for the full list.

If the build is successful, the 'build' folder will contain:

- `xSTUDIO-<version>-win64.zip` - a **portable** build (see below). Always produced.
- `xSTUDIO-<version>-win64.exe` - the installer. Only produced when NSIS is installed.

### Portable build (no installer)

The ZIP contains a self-contained xSTUDIO: Qt, ffmpeg, Python and the Microsoft C++ runtime are bundled. To use it on any Windows 10 (1903 or later) / Windows 11 PC:

1. Unzip it anywhere (Desktop, USB stick, network share). No administrator rights are needed.
2. Run `bin\xstudio.exe` inside the unzipped folder.

To build only the ZIP, without NSIS:

    cmake --build build --target package

Differences from the installer: no Start-menu shortcut, and `.xst` session files are not associated with xSTUDIO (open them from within xSTUDIO, or with "Open with..."). Because the executable is not code-signed, Windows SmartScreen may show "Windows protected your PC" the first time; click "More info" then "Run anyway".

The target PC needs a GPU driver with OpenGL support (any current NVIDIA, AMD or Intel driver). Remote Desktop sessions and very old integrated GPUs may not work.

### Building with GitHub Actions

If you do not want to build locally, the workflow `.github/workflows/windows-portable.yml` builds the portable ZIP on a GitHub-hosted Windows machine. In your fork on GitHub, open the "Actions" tab, select "Windows portable build" and click "Run workflow". When it finishes, download the `xSTUDIO-win64-portable` artifact from the run page. The first run compiles all dependencies and may take several hours; if it times out, run it again, as finished dependencies are cached.

To publish a GitHub Release instead, push a tag starting with `v` (for example `git tag v1.3.0-win && git push origin v1.3.0-win`). The workflow then attaches the ZIP and installer to a new Release, with notes linking to the exact source commit.

Builds made by this workflow are labelled as unofficial (`-DXSTUDIO_UNOFFICIAL_BUILD=ON`): the package vendor is "Unofficial community build" and a `README_UNOFFICIAL.txt` is added. Every Windows package includes a `licenses` folder with xSTUDIO's `LICENSE` and `NOTICE.TXT`, the GPL v3 and LGPL v3 texts, `THIRD_PARTY.md`, and the copyright file of each bundled vcpkg library.

### Running xSTUDIO from the build tree (dev workflow)

For a quick dev run without going through the installer, the build generates a launcher at `build/run_xstudio.bat`. Arguments are forwarded to xstudio:

    .\build\run_xstudio.bat path\to\session.xst
