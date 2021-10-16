# PyInstaller Docker Images

**batonogov/pyinstaller-linux** and **batonogov/pyinstaller-windows** are a pair of Docker containers to ease compiling Python applications to binaries / exe files.

Current PyInstaller version used: 
- 4.5.1 for Python 3.9.7

## Tags

`batonogov/pyinstaller-linux` both have few tags `:latest`, `:dev`, `:i386`, `:i386dev`. 
`batonogov/pyinstaller-windows` both have few tags `:latest`, `:dev`, `:win32`, `:win32dev`.

The `:latest` tag runs Python 3.9.7 x86-64.

## Usage

There are two containers, one for Linux and one for Windows builds. The Windows builder runs Wine inside Ubuntu to emulate Windows in Docker.

To build your application, you need to mount your source code into the `/src/` volume.

The source code directory should have your `.spec` file that PyInstaller generates. If you don't have one, you'll need to run PyInstaller once locally to generate it.

If the `src` folder has a `requirements.txt` file, the packages will be installed into the environment before PyInstaller runs.

For example, in the folder that has your source code, `.spec` file and `requirements.txt`:

```
docker run -v "$(pwd):/src/" batonogov/pyinstaller-windows
```

will build your PyInstaller project into `dist/`. The `.exe` file will have the same name as your `.spec` file.

```
docker run -v "$(pwd):/src/" batonogov/pyinstaller-linux
```

will build your PyInstaller project into `dist/`. The binary will have the same name as your `.spec` file.

##### How do I install system libraries or dependencies that my Python packages need?

You'll need to supply a custom command to Docker to install system pacakges. Something like:

```
docker run -v "$(pwd):/src/" --entrypoint /bin/sh batonogov/pyinstaller-linux -c "apt-get update -y && apt-get install -y wget && /entrypoint.sh"
```

Replace `wget` with the dependencies / package(s) you need to install.

##### How do I generate a .spec file?

`docker run -v "$(pwd):/src/" batonogov/pyinstaller-linux "pyinstaller your-script.py"`

will generate a `spec` file for `your-script.py` in your current working directory. See the PyInstaller docs for more information.

##### How do I change the PyInstaller version used?

Add `pyinstaller=4.1` to your `requirements.txt`.

##### Is it possible to use a package mirror?

Yes, by supplying the `PYPI_URL` and `PYPI_INDEX_URL` environment variables that point to your PyPi mirror.

## Known Issues

None

## History

#### [1.0] - 2016-08-26
First release, works.

#### [1.1] - 2016-12-13
Added Python 3.4 on Windows, thanks to @bmustiata

#### [1.2] - 2016-12-13
Added Python 3.5 on Windows, thanks (again) to @bmustiata

#### [1.3] - 2017-01-23
Upgraded PyInstaller to version 3.2.1.
Thanks to @bmustiata for contributing:
 - Custom PyPi URLs
 - No longer need to supply a requirements.txt file if your project doesn't need it
 - PyInstaller can be called directly, for e.g to generate a spec file

#### [1.4] - 2017-01-26
Fixed bug with concatenated commands in entrypoint arguments, thanks to @alph4

#### [1.5] - 2017-09-29
Changed the default PyInstaller version to 3.3

#### [1.6] - 2017-11-06
Added Python 3.6 on Windows, thanks to @jameshilliard

#### [1.7] - 2018-10-02
Bumped Python version to 3.6 on Linux, thank you @itouch5000

#### [1.8] - 2019-01-15
- Build using an older version of glibc to improve compatibility, thank you @itouch5000
- Updated PyInstaller to version 3.4

#### [1.9] - 2020-01-14
- Added a 32bit package, thank you @danielguardicore
- Updated PyInstaller to version 3.6

#### [2.0] - 2021-03-11
- Drop support for Python 2.7
- Updated Python 3 to version 3.9
- Updated PyInstaller to version 4.2

#### [2.1] - 2021-09-24
- Update Ubuntu 16:04 -> 20:04 for win64 and win32
- Updated PyInstaller 4.2 -> 4.5.1
- Updated Python 3.9.5 -> 3.9.7

#### [2.2] - 2021-10-17
- Update Ubuntu 12:04 -> 20:04 for amd64
- Update openssl 1.0.2u -> 1.1.1l for amd64
- Deleted Python 2 Dockerfiles
- Deleted 32 bit Dockerfiles

## License

MIT
