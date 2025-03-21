# Tinfoil Security Jenkins Plugin
This Jenkins plugin allows you to run web security tests from the cloud using Tinfoil Security.

## End-Of-Life

This plugin was deprecated in March 2025.

## Development

1. Install the latest and greatest JDK from Oracle. http://www.oracle.com/technetwork/java/javase/downloads/index.html

2. Install maven.  With homebrew: `$ brew install maven`

3. Configure maven's `settings.xml` (likely in ~/.m2/settings.xml). Instructions are here: https://wiki.jenkins-ci.org/display/JENKINS/Plugin+tutorial#Plugintutorial-SettingUpEnvironment

4. Make sure the project builds without Eclipse first. From the root of this repo, run the following: `mvn package`

5. Download Eclipse for Java. With homebrew: `$ brew cask install eclipse-java`

6. Open Eclipse and make a workspace. Then run the following command (which is really two commands in one) from the repo root to a) sync the maven dependencies with the eclipse classpath and b) make this project compatible with your Eclipse. Be sure to replace `/PATH/TO/YOUR/WORKSPACE`.
```$ mvn -DdownloadSources=true -DdownloadJavadocs=true -DoutputDirectory=target/eclipse-classes -Declipse.workspace=/PATH/TO/YOUR/WORKSPACE eclipse:eclipse eclipse:add-maven-repo```

7. Import this project into Eclipse using File > Import... > General > Existing Projects Into Workspace

Make sure you have your M2_REPO classpath variable set properly. Mine wasn't set at all.

* Open the Eclipse Preferences menu
* Go to [Java - Build Path - Classpath Variables]
* Click 'New' and set its name as M2_REPO
* Click 'Folder' and select your Maven repository folder `(~/.m2/repository)`.

Eclipse will prompt you to rebuild the project. Make sure that this returns no build errors. Warnings are OK.

If, after performing these steps, there are compilation errors in the test package, right click on the project in the workspace and click "Refresh".

## Testing

```$ mvn hpi:run```

This will run Jenkins with your Tinfoil plugin as an available plugin. Now you can install the plugin and use it. Jenkins by default runs on port 8080. If you need a different port, use the `jetty.port` option.

```$ mvn hpi:run -Djetty.port=8090```

## Building & Releasing

Notes can be found on the [Jenkins Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Hosting+Plugins#HostingPlugins-Releasingtojenkinsci.org)

1. `$ mvn release:prepare release:perform`

## Adding a library

Dependency management is handled by Maven. The file that stores dependencies is `pom.xml`. Never write to this file directly. Use Eclipse's editor instead.

After adding a dependency to the `pom.xml`, you need to do two things to get Eclipse to understand that it exists.

1. In the project root directory, run `$ mvn eclipse:eclipse`
2. In Eclipse, hit F5 to refresh.
