Parent Pom
==========

<a href="https://raw.githubusercontent.com/ArpNetworking/arpnetworking-parent-pom/master/LICENSE">
    <img src="https://img.shields.io/hexpm/l/plug.svg"
         alt="License: Apache 2">
</a>
<a href="https://travis-ci.com/ArpNetworking/arpnetworking-parent-pom">
    <img src="https://travis-ci.com/ArpNetworking/arpnetworking-parent-pom.svg?branch=master"
         alt="Travis Build">
</a>
<a href="http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.arpnetworking.build%22%20a%3A%22arpnetworking-parent-pom%22">
    <img src="https://img.shields.io/maven-central/v/com.arpnetworking.build/arpnetworking-parent-pom.svg"
         alt="Maven Artifact">
</a>

A parent pom for projects.


Leveraging the Parent Pom
-------------------------

### Add Dependency

Determine the latest version of the parent pom in [Maven Central](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.arpnetworking.build%22%20a%3A%22arpnetworking-parent-pom%22).

#### Maven

Set the parent to your pom:

```xml
<parent>
    <groupId>com.arpnetworking.build</groupId>
    <artifactId>arpnetworking-parent-pom</artifactId>
    <version>VERSION</version>
</parent>
```

The Maven Central repository is included by default.

### Checkstyle

The checkstyle configuration is obtained from the [build-resources]() package.  The checkstyle mojo is executed as part of the __verify__ lifecycle.  However, the mojo can also be executed directly.  In order to execute the checkstyle mojo directly you need to also ensure the build-resources are extracted.

    project> mvn dependency:unpack-dependencies checkstyle:check

### Maven Wrapper

The parent pom enables the use of [Maven Wrapper](https://github.com/rimerosolutions/maven-wrapper) in child projects and it also uses Maven Wrapper itself. In order to ensure consistent builds you should invoke __mvnw__ instead of __mvn__ to build the project locally and from continuous integration.

To allow developers to build wrapper and unwrapper projects seamlessly you can use this shell script to invoke either __mvnw__ or __mvn__ depending on which is available. Simply copy the script, make it executable and ensure it is first on __PATH__. Now continue to invoke __mvn__ and it will actually run __mvnw__ if available in the current project.

```bash
#!/bin/bash

JDKW_OPTIONS=""

# Look for maven wrapper
MVN_COMMAND=""
if [ -f "./mvnw" ]; then
  echo "Maven wrapper found; invoking mvnw"
  MVN_COMMAND="./mvnw"
else
  echo "Maven wrapper not found; invoking mvn"
  MVN_COMMAND="/usr/local/bin/mvn"
fi

# Look for jdk wrapper
if [ -f "jdk-wrapper.sh" ]; then
  MVN_COMMAND="./jdk-wrapper.sh ${JDKW_OPTIONS} ${MVN_COMMAND}"
elif [ -f ".jdkw" ]; then
  JDKW_REMOTE="https://raw.githubusercontent.com/vjkoskela/jdk-wrapper/master/jdk-wrapper.sh"
  JDKW_LOCAL="${HOME}/.jdk/jdk-wrapper.sh"
  mkdir -p "$(dirname "${JDKW_LOCAL}")"
  if [ -f "${JDKW_LOCAL}" ]; then
    curl "${JDKW_REMOTE}" -z "${JDKW_LOCAL}" -o "${JDKW_LOCAL}" --silent --location
  else
    curl "${JDKW_REMOTE}" -o "${JDKW_LOCAL}" --silent --location
  fi
  chmod +x "${JDKW_LOCAL}"
  MVN_COMMAND="${JDKW_LOCAL} ${JDKW_OPTIONS} ${MVN_COMMAND}"
fi

# Execute
eval "${MVN_COMMAND}" "$@"
exit $?
```

Please note that your environment should have JAVA_HOME set correctly.

Building
--------

Prerequisites:
* [JDK8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (Or Invoke with [JDK Wrapper](https://github.com/KoskiLabs/jdk-wrapper))

Building:

    arpnetworking-parent-pom> ./jdk-wrapper.sh ./mvnw verify

To use the local version you must first install it locally:

    arpnetworking-parent-pom> ./jdk-wrapper.sh ./mvnw install

You can determine the version of the local build from the pom file.  Using the local version is intended only for testing or development.

License
-------

Published under Apache Software License 2.0, see LICENSE

&copy; Arpnetworking Inc., 2015
