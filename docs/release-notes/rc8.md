## 1.0-RC8

**KNOWN ISSUES**

* Support for isolates in native-image is still experimental. A problem with garbage collection that can lead to stability issues has been identified and will be fixed in an upcoming release.

* The GraalVM CE image for MacOS currently depends on some libraries that are not installed by default on current MacOS versions.
This might cause issues with UI-related functionality:
```
Library not loaded: /usr/X11/lib/libfreetype.6.dylib
```
The necessary components can be added, e.g., by installing [https://www.xquartz.org](https://www.xquartz.org). We will remove this dependency in upcoming versions of GraalVM CE for MacOS.
* Due do the issue with the underlying platform, Java Mission control freezes at startup on MacOS. Because of that we removed the `jmc` utility from the distribution. For more information and workarounds please see the [JMC known issues page](https://www.oracle.com/technetwork/java/javase/jmc55-release-notes-2412446.html#known-iss).

### GraalVM for Java Developers (GraalVM + compiler)
* Added the support for Intel bit manipulation instructions. For more details, see [#666](https://github.com/oracle/graal/pull/666).
* Virtualize `unsafe` compare and swap calls on non-escaping objects. See [#636](https://github.com/oracle/graal/pull/636).

### Maven Artifacts
* The **`com.oracle.truffle`** group ID was renamed to **`org.graalvm.truffle`**.
* The `graal-sdk`, `launcher-common` and `polyglot-tck` artifacts were moved from the **`org.graalvm`** group ID to **`org.graalvm.sdk`**.
* New artifacts that are now available:
  - `org.graalvm.compiler:compiler`
  - `org.graalvm.js:js`
  - `org.graalvm.js:js-launcher`
  - `org.graalvm.js:js-scriptengine`
  - `org.graalvm.regex:regex`
  - `org.graalvm.tools:chromeinspector`
  - `org.graalvm.tools:profiler`
  - `org.graalvm.truffle:truffle-nfi`
  - `com.oracle.substratevm:library-support`
  - `com.oracle.substratevm:objectfile`
  - `com.oracle.substratevm:pointsto`
  - `com.oracle.substratevm:svm-driver`
  - `com.oracle.substratevm:svm`
* The artifacts that are now modular JARs:
  - `truffle-api`
  - `graal-sdk`
  - `js`
  - `js-scriptengine`
  - `compiler`
* The `compiler` artifact provides the **`jdk.internal.vm.compiler`** module and can be used to upgrade that module in JDK 11. Unlike the module present in the JDK, this version contains the optimizing Truffle runtime.

### Native Image Generation

* The native image generator now has automatic support for services loaded using ServiceLoader.
All service implementation classes, listed in the `META-INF` directory, are available automatically as soon as the service interface is used. This eliminates the need to manually register resources and reflection support for such classes. The automatic registration can be disabled with the `-H:-UseServiceLoaderFeature` option.
* Finished the support for isolates (starting multiple independent VM instances at run time) and compressed references (to reduce memory footprint; Enterprise Edition only). More details are in an upcoming blog article.
* A new Maven plugin allows native image generation from within a Maven build.
* All components necessary for native image generation (Substrate VM and the Graal compiler) are now available on Maven Central. The dependency for that is:
```
    <dependency>
      <groupId>com.oracle.substratevm</groupId>
      <artifactId>svm</artifactId>
      <version>1.0.0-rc8</version>
      <scope>provided</scope>
    </dependency>
```    

### JavaScript
* Updated Node.js to version 10.9.0.

A complete [changelog](https://github.com/graalvm/graaljs/blob/master/CHANGELOG.md) is available on GitHub.

### LLVM Interpreter (Sulong)
With GraalVM 1.0-rc8 Enterprise Edition the LLVM Interpreter announced:
* Better exceptions for memory access violations in sandboxed execution make it easier to diagnose the underlying problem. For example, an invalid access is now reported as Illegal pointer access `0x0000000000000001` instead of ERROR` java.lang.UnsupportedOperationException org.graalvm.polyglot.PolyglotException` (note that these are proper exceptions that can be caught and handled).
* Source-level debugging of C/C++/... code is now supported when running bitcode in sandboxed mode.

### Ruby
* Ubuntu 18.04 LTS, Fedora 28, and macOS 10.14 (Mojave) now supported.
* `Java.synchronized(object) { }` and `TruffleRuby.synchronized(object) { }` methods have been added.
* Added a `TruffleRuby::AtomicReference` class.
* Performance of setting the last exception on a thread has now been improved.

### Python
* Python now supports the allocation profiler (`--memtracer`) to analyze the heap usage of applications.

The [changelog](https://github.com/graalvm/graalpython/blob/master/CHANGELOG.md) is available on GitHub.

### R
GraalVM 1.0-rc8 implementation of R came up with multiple bug fixes which can be found in the project [changelog](https://github.com/oracle/fastr/blob/master/CHANGELOG.md) on GitHub.

### Tools
**Graal VisualVM**
* Introduced the CPU and Memory Sampler for guest languages.
* Submitted improvements in the R language heapwalker.

### API Changes for GraalVM Integrators (SDK + Truffle)
The major Truffle and Graal SDK changes between GraalVM versions are summarized in the project changelogs:
- [Graal SDK changelog](https://github.com/oracle/graal/blob/master/sdk/CHANGELOG.md)
- [Truffle changelog](https://github.com/oracle/graal/blob/master/truffle/CHANGELOG.md)