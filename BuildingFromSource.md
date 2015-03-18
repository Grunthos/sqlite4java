# Building from Source #

## Install Required SDKs and Libraries ##

You will need:
  * JDK 1.5+
    * In order to run tests, JRE matching target platform is needed; that is, to build and test sqlite4java for Windows x86 and x64, you'll need x86 and x64 versions of the JRE. It's possible to skip tests.
    * Install system package, or
    * Download <a href='http://www.oracle.com/technetwork/java/javase/downloads/index.html'>Java SE</a>
  * Apache Ant version 1.7.0 or later.
    * Install system package, or
    * Download from <a href='http://ant.apache.org/bindownload.cgi'>Apache website</a>
    * Make sure you have ANT\_HOME environment variable set to the Ant installation directory
    * If tests fail to start, try putting `junit.jar` from `lib/` folder in the source tree into the ANT\_HOME/lib folder.
  * Gant version 1.6.1 or later (with Groovy 1.6.0 or later)
    * Install system packages, or
    * Get older (but known to work) version here: <a href='http://almworks.com/sqlite4java/groovy-binary-1.6.0.zip'>Groovy</a> and <a href='http://almworks.com/sqlite4java/gant-1.6.1_groovy-1.6.0.zip'>Gant</a>, or
    * Get newer versions from Codehaus: <a href='http://groovy.codehaus.org/'>Groovy</a> and <a href='http://gant.codehaus.org/'>Gant</a>.
    * Make sure you have GROOVY\_HOME and GANT\_HOME environment variables set to the installation directories of Groovy and Gant, respectively.
  * SWIG 1.3.32 or later
    * Install system packages, or
    * Download from <a href='http://www.swig.org/download.html'>SWIG</a> website (Windows users should download `swigwin`)
  * On Windows: Windows SDK with VC compiler
    * If you don't have Windows SDK or Visual Studio installed, <a href='http://msdn.microsoft.com/en-us/windows/bb980924.aspx'>download Windows SDK from MSDN</a> (free).
  * On Linux and Mac OS X: GCC (optionally, with multi-platform support)
    * Install system packages (including `gcc-multilib` if you're building for a different platform)

## Check Out Sources ##

Get sources with `svn checkout http://sqlite4java.googlecode.com/svn/trunk/ sqlite4java`

The source tree contains the following directories:
  * `ant` - build scripts and related files;
  * `idea` - IntelliJ IDEA project files;
  * `java` - Java sources;
  * `lib` - libraries used during build;
  * `native` - additional C code for the wrapper;
  * `sqlite` - SQLite source files (amalgamation);
  * `swig` - source files for SWIG, with the pre-generated sources;
  * `test` - Java test sources.

## Run Build Script ##

Build script is `ant/build.gant`. You should run `gant` in the `ant/` subdirectory with at least one target and passing the required and maybe optional properties. Use target `all` if in doubt.

| **Target** | **Effect** |
|:-----------|:-----------|
| `dist` | Builds distributable files in `build/dist` directory |
| `all` | Builds and tests the library, compiles `build/distzip/sqlite4java-OS-NNN.zip`, the distributable file for your operating system |
| `test` | Runs tests |
| `clean` | Clears `build/` directory |
| `debug` | Tells script to build native libraries in DEBUG configuration only |
| `release` | Tells script to build native libraries in RELEASE configuration only |

Properties are passed with `-Dname=value` parameter.

| **Property** | **When Required?** | **Value means** |
|:-------------|:-------------------|:----------------|
| `jdk.home` | Always | Home path of the JDK used to compile the Java code |
| `swig.home` | If on Windows, or if `swig` is not on the `$PATH` | Home path of the SWIG code generator |
| `wsdk.home` | On Windows | Home path of the Windows SDK. There should be a file `bin\SetEnv.Cmd` in that directory that sets up the environment for VC build. |
| `platforms` | Never | Used to override the default list of platforms built under the current operating systems. See default values in `ant/operatingSystem.properties`. |
| `jre.win32-x86` | On Windows, if building with tests for x86 platform | JRE or JDK home path with the x86 Java |
| `jre.win32-x64` | On Windows, if building with tests for x64 platform | JRE or JDK home path with the x64 Java |
| `jre.linux-i386` | On Linux, if building with tests for i386 platform | JRE or JDK home path with the i386 Java |
| `jre.linux-amd64` | On Linux, if building with tests for amd64 platform | JRE or JDK home path with the amd64 Java |
| `jre.osx` | On Mac OS X, if building with tests | JRE or JDK home path |

## Sample Build Commands ##

On Windows:

> `gant -Djdk.home=C:\Dev\jdk16u17 -Dswig.home=C:\Dev\swigwin-1.3.32 -Dwsdk.home=C:\Dev\wsdk60 -Djre.win32-x86=C:\Dev\jdk16u17\jre -Djre.win32-x64=C:\Dev\jdk16u17x64\jre release all`

On Linux:

> `gant -Djdk.home=/opt/jdk -Djre.linux-i386=/opt/jdk/jre release all`





