# Installation

## Download

The latest _PraxisLIVE_ release can be downloaded from
[http://www.praxislive.org/download](http://www.praxislive.org/download). Details of each release and changes
included are on the [Github release page](https://github.com/praxis-live/praxis-live/releases).

Platform-specific packages are available for Windows, OS X and Linux. There is also a cross-platform Zip bundle.

## System requirements

PraxisLIVE packages for Windows, Linux and macOS include their own copy of OpenJDK.
Those using the plain zip download need to ensure they run with a Java 21 or higher JDK.

GStreamer 1.x is required for video capture and playback. The GStreamer libraries are not included in the
PraxisLIVE downloads to reduce their size and ensure you can always use the GStreamer version you want.

<!-- Latest recommended download links for GStreamer on Windows and macOS are on the Start Page in the application. Or
see [https://gstreamer.freedesktop.org/download/](https://gstreamer.freedesktop.org/download/) -->

## Windows

Download and run the installer. You may need to give it permission to make changes to your system.
The installer will (by default) create a menu entry and desktop shortcut.

For video playback and capture, install the GStreamer library. Make sure to choose the full installation,
rather than typical, if you want to use webcams or other capture devices.

## Linux

_PraxisLIVE_ is available as a Linux DEB package.

In the unlikely event you don't have GStreamer 1.x installed and require video playback and capture, 
install it from your distribution's repositories.

## macOS

_PraxisLIVE_ is available as a macOS Application Bundle. Simply download and extract the bundle from the archive.

For video playback and capture, install the GStreamer library.

## Zip bundle

A cross-platform Zip bundle is also available. This can be used if you cannot or do not want to install
_PraxisLIVE_ on your system, or wish to use the application with an alternative JDK.

To use the Zip bundle, unzip the archive to a suitable location, then run `praxislive` (Linux / macOS) or
`praxislive.exe` (Windows) from inside the `bin` directory. Make sure you have a Java 21+ JDK
installed and set as the default.
