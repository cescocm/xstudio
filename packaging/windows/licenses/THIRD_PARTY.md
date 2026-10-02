# Third-party software in this xSTUDIO build

This Windows build of xSTUDIO bundles the third-party software listed below.
xSTUDIO itself is licensed under the Apache License 2.0 (`LICENSE`); see also `NOTICE.TXT`.

## License texts

- `LICENSE` - Apache License 2.0 (xSTUDIO, OpenSSL, OpenTimelineIO and others)
- `GPL-3.0.txt` - GNU General Public License v3
- `LGPL-3.0.txt` - GNU Lesser General Public License v3
- `third_party/<library>/copyright` - the license file shipped with each library
  (included in builds made by the GitHub Actions workflow)

## GPL notice

The bundled FFmpeg is built with x264 and x265 (GPL-2.0-or-later) and with
`--enable-gpl --enable-version3`, so FFmpeg as distributed here is licensed under the
GNU GPL v3. The complete corresponding source code is available as follows:

- xSTUDIO source, including the build scripts: the commit of the fork from which this
  build was made, linked from the release page.
- All other libraries, including FFmpeg, x264 and x265: obtained and built by vcpkg
  (https://github.com/microsoft/vcpkg) at commit
  `c2aeddd80357b17592e59ad965d2adf65a19b22f`, with the versions pinned in `vcpkg.json`
  of the xSTUDIO source. Each library's upstream source is listed in the table below.

## Qt

This build uses the Qt 6.5.3 open-source edition under the GNU LGPL v3 (`LGPL-3.0.txt`).
The Qt libraries are separate DLLs in `bin\`, so they can be replaced with compatible
versions. Qt source code: https://download.qt.io/archive/qt/6.5/6.5.3/single/

## Python packages

The embedded Python in `bin\python3` includes packages installed with pip (for example
numpy, PyYAML, fileseq, OpenTimelineIO-Plugins). Each package's license is in its
`*.dist-info` folder under `bin\python3\Lib\site-packages`.

## Libraries

Generated from the vcpkg port manifests at the commit above, for the x64-windows target.
Licenses marked `*` are not declared in the vcpkg manifest and were taken from the
upstream project. The list may include build-only or header-only packages that are not
shipped as DLLs.

| Library | Version | License | Source |
|---|---|---|---|
| aom | 3.13.1 | BSD-2-Clause | https://aomedia.googlesource.com/aom |
| bzip2 | 1.0.8 | bzip2-1.0.6 | https://sourceware.org/bzip2/ |
| caf | 1.0.2 | BSD-3-Clause | https://github.com/actor-framework/actor-framework |
| egl-registry | 2025-05-27 | Apache-2.0 | https://github.com/KhronosGroup/EGL-Registry |
| expat | 2.7.3 | MIT | https://github.com/libexpat/libexpat |
| ffmpeg | 7.1.1 | GPL-3.0-or-later (this build: --enable-gpl --enable-version3) | https://ffmpeg.org |
| fmt | 12.1.0 | MIT | https://github.com/fmtlib/fmt |
| freetype | 2.10.1-6 | FTL OR GPL-2.0-or-later * | https://www.freetype.org/ |
| glew | 2.3.1 | BSD-3-Clause AND MIT * | https://github.com/nigels-com/glew |
| imath | 3.2.2 | BSD-3-Clause | https://github.com/AcademySoftwareFoundation/Imath |
| lcms | 2.18 | MIT | https://github.com/mm2/Little-CMS |
| libdeflate | 1.25 | MIT | https://github.com/ebiggers/libdeflate |
| libffi | 3.5.2 | MIT | https://github.com/libffi/libffi |
| libiconv | 1.18 | LGPL-2.1-or-later * | https://www.gnu.org/software/libiconv/ |
| libjpeg-turbo | 3.1.3 | BSD-3-Clause | https://github.com/libjpeg-turbo/libjpeg-turbo |
| liblzma | 5.8.2 | 0BSD * | https://tukaani.org/xz/ |
| libogg | 1.3.6 | BSD-3-Clause | https://www.xiph.org/ogg |
| libopenmpt | 0.7.13 | BSD-3-Clause | https://openmpt.org/ |
| libpng | 1.6.54 | libpng-2.0 | https://github.com/pnggroup/libpng |
| libssh | 0.11.3 | LGPL-2.1-only | https://www.libssh.org/ |
| libtheora | 1.2.0 | BSD-3-Clause * | https://github.com/xiph/theora |
| libvorbis | 1.3.7 | BSD-3-Clause | https://github.com/xiph/vorbis |
| libvpx | 1.15.2 | BSD-3-Clause | https://github.com/webmproject/libvpx |
| libwebp | 1.6.0 | BSD-3-Clause | https://github.com/webmproject/libwebp |
| libxml2 | 2.15.1 | MIT | https://gitlab.gnome.org/GNOME/libxml2/-/wikis/home |
| minizip-ng | 4.1.0 | Zlib | https://github.com/zlib-ng/minizip-ng |
| mp3lame | 3.100 | LGPL-2.0-only | https://lame.sourceforge.io |
| mpg123 | 1.33.4 | LGPL-2.1-or-later | https://sourceforge.net/projects/mpg123/ |
| nlohmann-json | 3.12.0 | MIT | https://github.com/nlohmann/json |
| opencolorio | 2.2.1 | BSD-3-Clause | https://opencolorio.org/ |
| openexr | 3.4.4 | BSD-3-Clause | https://www.openexr.com/ |
| opengl | 2022-12-04 | (system library, not bundled) | - |
| opengl-registry | 2025-10-23 | Apache-2.0 / MIT (headers only) * | https://github.com/KhronosGroup/OpenGL-Registry |
| openimageio | 3.0.9.1 | BSD-3-Clause | https://github.com/OpenImageIO/oiio |
| openjpeg | 2.5.4 | BSD-2-Clause | https://github.com/uclouvain/openjpeg |
| openjph | 0.26.0 | BSD-2-Clause | https://github.com/aous72/OpenJPH |
| openssl | 3.6.1 | Apache-2.0 | https://www.openssl.org |
| opentimelineio | 0.17.0 | Apache-2.0 | https://opentimeline.io |
| opus | 1.5.2 | BSD-3-Clause | https://github.com/xiph/opus |
| pybind11 | 3.0.1 | BSD-3-Clause | https://github.com/pybind/pybind11 |
| pystring | 1.1.4 | BSD-3-Clause | https://github.com/imageworks/pystring |
| python3 | 3.11.11 | Python-2.0 | https://github.com/python/cpython |
| reproc | 14.2.5 | MIT * | https://github.com/DaanDeMeyer/reproc |
| robin-map | 1.4.1 | MIT | https://github.com/Tessil/robin-map |
| snappy | 1.2.2 | BSD-3-Clause * | https://github.com/google/snappy |
| soxr | 0.1.3 | LGPL-2.1-or-later * | https://sourceforge.net/projects/soxr/ |
| spdlog | 1.17.0 | MIT | https://github.com/gabime/spdlog |
| sqlite3 | 3.51.2 | blessing | https://sqlite.org/ |
| stduuid | 1.2.3 | MIT | https://github.com/mariusbancila/stduuid |
| tiff | 4.7.1 | libtiff | https://libtiff.gitlab.io/libtiff/ |
| x264 | 0.164.3108 | GPL-2.0-or-later | https://www.videolan.org/developers/x264.html |
| x265 | 4.1 | GPL-2.0-or-later | https://bitbucket.org/multicoreware/x265_git/ |
| yaml-cpp | 0.8.0 | MIT | https://github.com/jbeder/yaml-cpp |
| zlib | 1.3.1 | Zlib | https://www.zlib.net/ |
| zstd | 1.5.7 | BSD-3-Clause OR GPL-2.0-only | https://facebook.github.io/zstd/ |
| Qt | 6.5.3 | LGPL-3.0-only | https://www.qt.io/ |
