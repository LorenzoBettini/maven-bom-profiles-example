# An example showing how to use profiles in a Maven BOM for dependency management according to the Java version 

Assuming you have several Java versions installed on your system.

The BOM configures a dependency (`org.eclipse.jdt.core`) with version `3.37.0` by default, which requires Java 17.
It has a profile that is automatically activated when the current Java version is LESS than 17: in that case, the version is configured as `3.33.0`, the latest version NOT requiring Java 17.

First, install the BOM locally:

```
mvn -f bom-example install
```

Then, build the example client project invoking the goal to copy resolved dependencies in the `lib` folder.

The client imports the BOM and has a dependency on `org.eclipse.jdt.core`.

Build with Java 17 (how to set the `JAVA_HOME` depends on your OS; my example relies on Arch):

```
JAVA_HOME=/usr/lib/jvm/java-17-openjdk mvn clean dependency:copy-dependencies@copy-libs -f bom-client-example
```

Inspect its `lib` folder, showing that `org.eclipse.jdt.core` has been resolved with version `3.37.0`, and its transitive dependencies accordingly:

```
ls bom-client-example/lib

ecj-3.37.0.jar                            org.eclipse.equinox.common-3.19.0.jar
org.eclipse.core.commands-3.12.0.jar      org.eclipse.equinox.preferences-3.11.0.jar
org.eclipse.core.contenttype-3.9.300.jar  org.eclipse.equinox.registry-3.12.0.jar
org.eclipse.core.expressions-3.9.300.jar  org.eclipse.jdt.core-3.37.0.jar
org.eclipse.core.filesystem-1.10.300.jar  org.eclipse.osgi-3.19.0.jar
org.eclipse.core.jobs-3.15.200.jar        org.eclipse.text-3.14.0.jar
org.eclipse.core.resources-3.20.100.jar   org.osgi.service.prefs-1.1.2.jar
org.eclipse.core.runtime-3.31.0.jar       osgi.annotation-8.0.1.jar
org.eclipse.equinox.app-1.7.0.jar
```

Now, build with Java 11:

```
JAVA_HOME=/usr/lib/jvm/java-11-openjdk mvn clean dependency:copy-dependencies@copy-libs -f bom-client-example
```

Inspect its `lib` folder, showing that `org.eclipse.jdt.core` has been resolved with version `3.33.0`, and its transitive dependencies accordingly:

```
ls bom-client-example/lib

ecj-3.33.0.jar                            org.eclipse.equinox.common-3.17.0.jar
org.eclipse.core.commands-3.10.300.jar    org.eclipse.equinox.preferences-3.10.100.jar
org.eclipse.core.contenttype-3.8.200.jar  org.eclipse.equinox.registry-3.11.200.jar
org.eclipse.core.expressions-3.8.200.jar  org.eclipse.jdt.core-3.33.0.jar
org.eclipse.core.filesystem-1.9.500.jar   org.eclipse.osgi-3.18.300.jar
org.eclipse.core.jobs-3.13.200.jar        org.eclipse.text-3.12.300.jar
org.eclipse.core.resources-3.18.200.jar   org.osgi.service.prefs-1.1.2.jar
org.eclipse.core.runtime-3.26.100.jar     osgi.annotation-8.0.1.jar
org.eclipse.equinox.app-1.6.200.jar
```
