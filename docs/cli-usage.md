# PraxisCORE command line launcher

The _PraxisCORE_ command line launcher can be found inside the `praxiscore/bin` folder inside
_PraxisLIVE_. The launcher is used by the IDE for launching projects and child processes.
The _PraxisCORE_ distribution can be copied from inside the IDE, or downloaded separately.
It can be used for launching projects from the command line, creating additional network
processes, and for creating standalone executable projects.

If you're using the Linux AppImage, _PraxisCORE_ can be launched by passing `praxis` as
the first argument to the AppImage file.

```
./praxislive-5.0.0-x86_64.AppImage praxis --port auto 

```

Use `--help` to see the full range of arguments available.

To create a standalone project just copy the `bin` and `mods` folders from _PraxisCORE_ into the project
folder. Optionally include a JDK in a `jdk` folder. The launcher files inside `bin` can
be renamed to whatever you like. The launcher will find and run the `project.pxp` file automatically.