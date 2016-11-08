# Installation

## Download

The latest _Praxis LIVE_ can be downloaded from [http://www.praxislive.org/download](http://www.praxislive.org/download)

Platform-specific packages are available for Windows, OS X and Linux. There is also a cross-platform Zip bundle.

## System requirements

_Praxis LIVE_ requires Java 8 or above, either Oracle's distribution or OpenJDK.

GStreamer 1.x is required for video capture and playback. Since v3 the GStreamer libraries are not included in the Praxis LIVE downloads for Windows and OSX - this is to reduce their size and ensure you can always use the GStreamer version you want. Latest recommended download links for GStreamer on Windows and OSX will be shown on the **Start Page** in the application and on the [GitHub releases page](https://github.com/praxis-live/praxis-live/releases).

## Windows

**If you have an earlier version of Praxis LIVE already on your system then you should uninstall this before installing the new version**. The uninstaller can be found in the directory in which you installed _Praxis LIVE_ (you can also uninstall from the control panel). It is advisable to select the option to delete the existing user directory.

Download and run the installer. You may need to give it permission to make changes to your system. The installer will (by default) create a menu entry and desktop shortcut, and offer to start _Praxis LIVE_ directly after installation.

## OS X

_Praxis LIVE_ is available as an OS X Application Bundle. Simply download and extract the bundle from the Zip archive. **Make sure you already have Java 8 installed or the bundle will fail to run**.

## Linux

The Linux `.deb` package should work on any Debian / Ubuntu derived system. Installation requires admin permissions, and can be managed like any other software on your system. The package file should open by default in GDebi or Ubuntu Software Centre. GStreamer 1.x will be recommended in the unlikely event it is not already installed.

## Zip bundle

A cross-platform Zip bundle is also available, which includes the GStreamer video libraries for both Windows and OS X. This can be used if you cannot install _Praxis LIVE_ on your system, though installed usage should be preferred.

To use the Zip bundle, unzip the archive to a suitable location, then run `praxis_live` (Linux / OS X) or `praxis_live.exe` (Windows) from inside the `bin` directory.


