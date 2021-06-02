# Libraries

Third-party libraries can be added to projects for component code to
utilise. Libraries can be added as local `*.jar` files, or by linking to
libraries from a remote repository (eg. Maven Central). Dependencies of remote
libraries will be automatically resolved and included.

To manage libraries configuration, right-click on the project root node in the
Projects tab, select `Properties` and use the `Libraries` section of the project
properties.

Libraries can be added even while a project is running. For technical reasons,
libraries can currently only be removed when the project is inactive.

## Import a local JAR file

To import a local JAR file, use the `Import` button in the libraries
configuration. Any files selected will be copied into the project folder, and
all JAR files added to the project classpath.

## Add a remote dependency

To add a remote dependency use the `Add` button in the libraries configuration.
These dependencies are referenced using a
[Package URL or PURL](https://github.com/package-url/purl-spec). This is a
concise format that also works with various aspects of the existing library
support that expect URIs.

A Maven PURL has the following format :

`pkg:maven/{group-id}/{artefact-id}@{version}`

The PURL for any library can be found in searching at
[https://search.maven.org](https://search.maven.org)
eg. the PURL for
[https://search.maven.org/artifact/org.apache.commons/commons-lang3/3.11/jar](https://search.maven.org/artifact/org.apache.commons/commons-lang3/3.11/jar)
is `pkg:maven/org.apache.commons/commons-lang3@3.11`

Only `pkg:maven` PURLs are supported at present, and query parameters are not
yet supported.

The Maven dependency resolution is currently based on
[Apache Ivy](https://ant.apache.org/ivy/). Cached dependencies are stored in the
default (`~/.ivy2/cache`) location.
