# Installation

## Download

The latest _Praxis LIVE_ release can be downloaded from [http://www.praxislive.org/download](http://www.praxislive.org/download). Details of each release and changes included are on the [Github release page](https://github.com/praxis-live/praxis-live/releases).

Platform-specific packages are available for Windows, OS X and Linux. There is also a cross-platform Zip bundle.

## System requirements

_Praxis LIVE_ requires Java 8, either OpenJDK (preferred) or Oracle's distribution. **Praxis LIVE v3.x will not currently work with Java 9.** Praxis LIVE is tested on Windows and OSX with [Azul's Zulu distribution](https://www.azul.com/downloads/zulu/) of OpenJDK. Praxis LIVE installers with an embedded JVM are currently in development.

GStreamer 1.x is required for video capture and playback. Since v3 the GStreamer libraries are not included in the Praxis LIVE downloads for Windows and OSX - this is to reduce their size and ensure you can always use the GStreamer version you want. Latest recommended download links for GStreamer on Windows and OSX are shown below and on the Start Page in the application.

## Windows

**If you have an earlier version of Praxis LIVE already on your system then you should uninstall this before installing the new version**. The uninstaller can be found in the directory in which you installed _Praxis LIVE_ (you can also uninstall from the control panel). It is advisable to select the option to delete the existing user directory.

Download and run the installer. You may need to give it permission to make changes to your system. The installer will (by default) create a menu entry and desktop shortcut, and offer to start _Praxis LIVE_ directly after installation.

For video playback and capture, install the GStreamer library. Make sure to choose the full installation, rather than typical, if you want to use webcams, etc. **Also note that if you're using Oracle's Java on a 64-bit Windows machine, there's a good chance the JVM is still 32-bit** - check in Praxis LIVE's `Help/About` window. Make sure to choose the installation of GStreamer that matches.

* 64-bit GStreamer installer - [https://gstreamer.freedesktop.org/data/pkg/windows/1.12.4/gstreamer-1.0-x86_64-1.12.4.msi](https://gstreamer.freedesktop.org/data/pkg/windows/1.12.4/gstreamer-1.0-x86_64-1.12.4.msi)
* 32-bit GStreamer installer - [https://gstreamer.freedesktop.org/data/pkg/windows/1.12.4/gstreamer-1.0-x86-1.12.4.msi](https://gstreamer.freedesktop.org/data/pkg/windows/1.12.4/gstreamer-1.0-x86-1.12.4.msi)

## OS X

_Praxis LIVE_ is available as an OS X Application Bundle. Simply download and extract the bundle from the archive. **Make sure you already have Java 8 installed or the app bundle will fail to run**.

For video playback and capture, install the GStreamer library.

* OSX GStreamer installer - [https://gstreamer.freedesktop.org/data/pkg/osx/1.12.4/gstreamer-1.0-1.12.4-x86_64.pkg](https://gstreamer.freedesktop.org/data/pkg/osx/1.12.4/gstreamer-1.0-1.12.4-x86_64.pkg)

## Linux

The Linux `.deb` package should work on any Debian / Ubuntu derived system. Installation requires admin permissions, and can be managed like any other software on your system. The package file should open by default in GDebi or Ubuntu Software Centre. GStreamer 1.x will be recommended in the unlikely event it is not already installed.

## Zip bundle

A cross-platform Zip bundle is also available. This can be used if you cannot install _Praxis LIVE_ on your system, though installed usage should be preferred.

To use the Zip bundle, unzip the archive to a suitable location, then run `praxis_live` (Linux / OS X) or `praxis_live.exe` (Windows) from inside the `bin` directory.


