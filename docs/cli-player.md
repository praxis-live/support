# Command line player

The `praxis` command line player is shipped in the same directory as the launcher for _PraxisLIVE_. It can be used to run a project directly from the command line without running the full application. For this usage the player takes a single default argument - a project directory or an individual script file (you can point it at the `.pxp` file in a project too). Make sure at least one root component in your project has the `.exit-on-stop` property set, or you run the command within an open terminal, otherwise the framework will still be running even when all windows have been closed!

The command line player is also used to launch slaves for use in a [distributed hub](distributed-hubs.md).
