# Installation

## Download

The latest _PraxisLIVE_ release can be downloaded from [http://www.praxislive.org/download](http://www.praxislive.org/download).
Details of each release and changes included are on the [Github release page](https://github.com/praxis-live/praxis-live/releases).

Platform-specific packages are available for Windows, OS X and Linux. There is also a cross-platform Zip bundle.

## System requirements

PraxisLIVE downloads for Windows and macOS include their own copy of OpenJDK 8.
Linux users, or those using the plain zip download, need to ensure they run with a
Java 8 JDK. **_PraxisLIVE_ requires Java 8, and will not currently run on Java 9 or above**
(support for running on Java 9+ will come in a v4 update).

GStreamer 1.x is required for video capture and playback. The GStreamer
libraries are not included in the PraxisLIVE downloads for. This is
to reduce their size and ensure you can always use the GStreamer version you want.
Latest recommended download links for GStreamer on Windows and macOS are shown below
and on the Start Page in the application.

The OpenJDK currently bundled for Windows and macOS is [Azul's Zulu distribution](https://www.azul.com/downloads/zulu/).

## Windows

**If you have an earlier version of PraxisLIVE already on your system then you should
uninstall this before installing the new version**. The uninstaller can be found in the
folder in which you installed _PraxisLIVE_ (you can also uninstall from the control panel).

Download and run the installer. You may need to give it permission to make changes to your system.
The installer will (by default) create a menu entry and desktop shortcut, and offer to start
_PraxisLIVE_ directly after installation.

For video playback and capture, install the GStreamer library. Make sure to choose the full installation, rather than typical, if you want to use webcams, etc.

* 64-bit GStreamer installer - [https://gstreamer.freedesktop.org/data/pkg/windows/1.12.4/gstreamer-1.0-x86_64-1.12.4.msi](https://gstreamer.freedesktop.org/data/pkg/windows/1.12.4/gstreamer-1.0-x86_64-1.12.4.msi)
* 32-bit GStreamer installer - [https://gstreamer.freedesktop.org/data/pkg/windows/1.12.4/gstreamer-1.0-x86-1.12.4.msi](https://gstreamer.freedesktop.org/data/pkg/windows/1.12.4/gstreamer-1.0-x86-1.12.4.msi)

## macOS

_PraxisLIVE_ is available as macOS Application Bundle. Simply download and extract
the bundle from the archive.

For video playback and capture, install the GStreamer library.

* OSX GStreamer installer - [https://gstreamer.freedesktop.org/data/pkg/osx/1.12.4/gstreamer-1.0-1.12.4-x86_64.pkg](https://gstreamer.freedesktop.org/data/pkg/osx/1.12.4/gstreamer-1.0-1.12.4-x86_64.pkg)

## Linux

The Linux `.deb` package should work on any Debian / Ubuntu derived system.
Installation requires admin permissions, and can be managed like any other software on your system.
The package file should open by default in GDebi or Ubuntu Software Centre.
GStreamer 1.x will be recommended in the unlikely event it is not already installed.

## Zip bundle

A cross-platform Zip bundle is also available. This can be used if you cannot install
_PraxisLIVE_ on your system, though installed usage should be preferred.

To use the Zip bundle, unzip the archive to a suitable location, then run `praxis_live`
(Linux / macOS) or `praxis_live.exe` (Windows) from inside the `bin` directory. Make
sure you have a Java 8 JDK installed and set as the default.


