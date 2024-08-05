## log4j2-v2231-graalvm-native-image-test

- For https://github.com/apache/hive/pull/5375 .

- Verified under Ubuntu 22.04.4 and `SDKMAN!`.

```shell
sdk install java 8.0.422-tem
sdk install java 22.0.2-graalce
sdk use java 8.0.422-tem
sdk install maven
git clone git@github.com:linghengqian/hive.git -b log4j-bump
cd ./hive/
mvn clean install -DskipTests
mvn clean package -pl packaging -DskipTests -Pdocker
cd ../

git clone git@github.com:linghengqian/log4j2-v2231-graalvm-native-image-test.git
cd ./log4j2-v2231-graalvm-native-image-test/
sdk use java 22.0.2-graalce
./mvnw -PgenerateMetadata -DskipNativeTests -e -T1C clean test native:metadata-copy
./mvnw -PnativeTestInHive -T1C -e clean test
```

- Log as follows.

```shell
$ ./mvnw -PnativeTestInHive -T1C -e clean test

========================================================================================================================
GraalVM Native Image: Generating 'native-tests' (executable)...
========================================================================================================================
Warning: Cannot register dynamic proxy for interface list: org.ehcache.shadow.org.terracotta.offheapstore.storage.portability.Portability, org.ehcache.shadow.org.terracotta.offheapstore.disk.persistent.PersistentPortability. Reason: Class org.ehcache.shadow.org.terracotta.offheapstore.storage.portability.Portability not found..
Warning: Cannot register dynamic proxy for interface list: org.ehcache.shadow.org.terracotta.offheapstore.storage.portability.WriteBackPortability, org.ehcache.shadow.org.terracotta.offheapstore.disk.persistent.PersistentPortability. Reason: Class org.ehcache.shadow.org.terracotta.offheapstore.storage.portability.WriteBackPortability not found..
[1/8] Initializing...                                                                                   (23.9s @ 0.94GB)
 Java version: 22.0.2+9, vendor version: GraalVM CE 22.0.2+9.1
 Graal compiler: optimization level: b, target machine: x86-64-v3
 C compiler: gcc (linux, x86_64, 11.4.0)
 Garbage collector: Serial GC (max heap size: 80% of RAM)
 3 user-specific feature(s):
 - com.oracle.svm.polyglot.groovy.GroovyIndyInterfaceFeature
 - com.oracle.svm.thirdparty.gson.GsonFeature
 - org.graalvm.junit.platform.JUnitPlatformFeature
------------------------------------------------------------------------------------------------------------------------
 1 experimental option(s) unlocked:
 - '-H:ReflectionConfigurationResources': Use a reflect-config.json in your META-INF/native-image/<groupID>/<artifactID> directory instead. (origin(s): 'META-INF/native-image/io.grpc.netty.shaded.io.netty/transport/native-image.properties' in 'file:///home/linghengqian/.m2/repository/io/grpc/grpc-netty-shaded/1.51.0/grpc-netty-shaded-1.51.0.jar')
------------------------------------------------------------------------------------------------------------------------
Build resources:
 - 10.19GB of memory (47.4% of 21.50GB system memory, determined at start)
 - 16 thread(s) (100.0% of 16 available processor(s), determined at start)
[junit-platform-native] Running in 'test listener' mode using files matching pattern [junit-platform-unique-ids*] found in folder [/home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/test-ids] and its subfolders.
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/home/linghengqian/.m2/repository/org/apache/logging/log4j/log4j-slf4j-impl/2.23.1/log4j-slf4j-impl-2.23.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/home/linghengqian/.m2/repository/org/slf4j/slf4j-reload4j/1.7.36/slf4j-reload4j-1.7.36.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
[2/8] Performing analysis...  [******]                                                                  (60.7s @ 4.16GB)
   32,728 reachable types   (84.4% of   38,777 total)
   60,523 reachable fields  (62.1% of   97,528 total)
  174,683 reachable methods (55.5% of  314,543 total)
   11,500 types, 1,871 fields, and 7,689 methods registered for reflection
      114 types,   212 fields, and   185 methods registered for JNI access
        4 native libraries: dl, pthread, rt, z
[3/8] Building universe...                                                                              (11.9s @ 3.76GB)
[4/8] Parsing methods...      [**]                                                                       (4.5s @ 4.37GB)
[5/8] Inlining methods...     [****]                                                                     (3.7s @ 4.74GB)
[6/8] Compiling methods...    [*****]                                                                   (31.3s @ 3.99GB)
[7/8] Laying out methods...   [****]                                                                    (13.5s @ 4.88GB)
[8/8] Creating image...       [***]                                                                      (9.1s @ 5.49GB)
  81.84MB (50.35%) for code area:   103,141 compilation units
  71.21MB (43.81%) for image heap:  647,176 objects and 342 resources
   9.48MB ( 5.83%) for other data
 162.53MB in total
------------------------------------------------------------------------------------------------------------------------
Top 10 origins of code area:                                Top 10 object types in image heap:
  16.10MB java.base                                           25.95MB byte[] for code metadata
  15.80MB hive-exec-4.1.0-SNAPSHOT.jar                        10.40MB byte[] for java.lang.String
   6.05MB java.xml                                             9.19MB java.lang.Class
   5.17MB testcontainers-1.20.1.jar                            6.46MB java.lang.String
   3.12MB groovy-all-2.4.21.jar                                2.75MB com.oracle.svm.core.hub.DynamicHubCompanion
   2.99MB hadoop-hdfs-client-3.3.6.jar                         1.99MB byte[] for embedded resources
   2.36MB java.desktop                                         1.98MB byte[] for reflection metadata
   1.95MB jackson-databind-2.16.1.jar                          1.36MB java.lang.String[]
   1.91MB hadoop-common-3.3.6.jar                              1.16MB c.o.svm.core.hub.DynamicHub$ReflectionMetadata
   1.88MB mssql-jdbc-6.2.1.jre7.jar                            1.12MB byte[] for general heap data
  23.77MB for 129 more packages                                8.86MB for 5020 more object types
------------------------------------------------------------------------------------------------------------------------
Recommendations:
 AWT:  Use the tracing agent to collect metadata for AWT.
 HEAP: Set max heap for improved and more predictable memory usage.
 CPU:  Enable more CPU features with '-march=native' for improved performance.
------------------------------------------------------------------------------------------------------------------------
                       17.3s (10.3% of total time) in 467 GCs | Peak RSS: 7.50GB | CPU load: 9.65
------------------------------------------------------------------------------------------------------------------------
Build artifacts:
 /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/libawt.so (jdk_library)
 /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/libawt_headless.so (jdk_library)
 /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/libawt_xawt.so (jdk_library)
 /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/libfontmanager.so (jdk_library)
 /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/libjava.so (jdk_library_shim)
 /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/libjavajpeg.so (jdk_library)
 /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/libjvm.so (jdk_library_shim)
 /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/liblcms.so (jdk_library)
 /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/native-tests (executable)
========================================================================================================================
Finished generating 'native-tests' in 2m 45s.
[INFO] Executing: /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/native-tests --xml-output-dir /home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/native-test-reports -Djunit.platform.listeners.uid.tracking.output.dir=/home/linghengqian/TwinklingLiftWorks/git/public/log4j2-v2231-graalvm-native-image-test/target/test-ids
JUnit Platform on Native Image - report
----------------------------------------

2024-08-05T05:18:53.726155Z main ERROR Unable to load services for service class org.apache.logging.log4j.spi.Provider java.lang.InternalError: com.oracle.svm.core.jdk.UnsupportedFeatureError: Defining hidden classes at runtime is not supported.
        at java.base@22.0.2/java.lang.invoke.InnerClassLambdaMetafactory.generateInnerClass(InnerClassLambdaMetafactory.java:367)
        at java.base@22.0.2/java.lang.invoke.InnerClassLambdaMetafactory.spinInnerClass(InnerClassLambdaMetafactory.java:289)
        at java.base@22.0.2/java.lang.invoke.InnerClassLambdaMetafactory.buildCallSite(InnerClassLambdaMetafactory.java:221)
        at java.base@22.0.2/java.lang.invoke.LambdaMetafactory.metafactory(LambdaMetafactory.java:341)
        at org.apache.logging.log4j.util.ServiceLoaderUtil.callServiceLoader(ServiceLoaderUtil.java:111)
        at org.apache.logging.log4j.util.ServiceLoaderUtil$ServiceLoaderSpliterator.<init>(ServiceLoaderUtil.java:151)
        at org.apache.logging.log4j.util.ServiceLoaderUtil.loadClassloaderServices(ServiceLoaderUtil.java:100)
        at org.apache.logging.log4j.util.ServiceLoaderUtil.loadServices(ServiceLoaderUtil.java:82)
        at org.apache.logging.log4j.util.ServiceLoaderUtil.loadServices(ServiceLoaderUtil.java:76)
        at org.apache.logging.log4j.util.ProviderUtil.<init>(ProviderUtil.java:73)
        at org.apache.logging.log4j.util.ProviderUtil.lazyInit(ProviderUtil.java:154)
        at org.apache.logging.log4j.util.ProviderUtil.hasProviders(ProviderUtil.java:138)
        at org.apache.logging.log4j.LogManager.<clinit>(LogManager.java:89)
        at org.apache.logging.slf4j.Log4jLoggerFactory.getContext(Log4jLoggerFactory.java:54)
        at org.apache.logging.log4j.spi.AbstractLoggerAdapter.getLogger(AbstractLoggerAdapter.java:46)
        at org.apache.logging.slf4j.Log4jLoggerFactory.getLogger(Log4jLoggerFactory.java:32)
        at org.slf4j.LoggerFactory.getLogger(LoggerFactory.java:363)
        at org.slf4j.LoggerFactory.getLogger(LoggerFactory.java:388)
        at org.testcontainers.containers.wait.strategy.HostPortWaitStrategy.<clinit>(HostPortWaitStrategy.java:26)
        at org.testcontainers.containers.wait.strategy.Wait.forListeningPort(Wait.java:25)
        at org.testcontainers.containers.wait.strategy.Wait.defaultWaitStrategy(Wait.java:15)
        at org.testcontainers.containers.GenericContainer.<clinit>(GenericContainer.java:184)
        at com.lingh.HiveTest.<init>(HiveTest.java:24)
        at java.base@22.0.2/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:502)
        at java.base@22.0.2/java.lang.reflect.Constructor.newInstance(Constructor.java:486)
        at org.junit.platform.commons.util.ReflectionUtils.newInstance(ReflectionUtils.java:553)
        at org.junit.jupiter.engine.execution.ConstructorInvocation.proceed(ConstructorInvocation.java:56)
        at org.junit.jupiter.engine.execution.InvocationInterceptorChain$ValidatingInvocation.proceed(InvocationInterceptorChain.java:131)
        at org.junit.jupiter.api.extension.InvocationInterceptor.interceptTestClassConstructor(InvocationInterceptor.java:74)
        at org.junit.jupiter.engine.execution.InterceptingExecutableInvoker.lambda$invoke$0(InterceptingExecutableInvoker.java:93)
        at org.junit.jupiter.engine.execution.InvocationInterceptorChain$InterceptedInvocation.proceed(InvocationInterceptorChain.java:106)
        at org.junit.jupiter.engine.execution.InvocationInterceptorChain.proceed(InvocationInterceptorChain.java:64)
        at org.junit.jupiter.engine.execution.InvocationInterceptorChain.chainAndInvoke(InvocationInterceptorChain.java:45)
        at org.junit.jupiter.engine.execution.InvocationInterceptorChain.invoke(InvocationInterceptorChain.java:37)
        at org.junit.jupiter.engine.execution.InterceptingExecutableInvoker.invoke(InterceptingExecutableInvoker.java:92)
        at org.junit.jupiter.engine.execution.InterceptingExecutableInvoker.invoke(InterceptingExecutableInvoker.java:62)
        at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.invokeTestClassConstructor(ClassBasedTestDescriptor.java:364)
        at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.instantiateTestClass(ClassBasedTestDescriptor.java:311)
        at org.junit.jupiter.engine.descriptor.ClassTestDescriptor.instantiateTestClass(ClassTestDescriptor.java:79)
        at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.instantiateAndPostProcessTestInstance(ClassBasedTestDescriptor.java:287)
        at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.lambda$testInstancesProvider$4(ClassBasedTestDescriptor.java:279)
        at java.base@22.0.2/java.util.Optional.orElseGet(Optional.java:364)
        at org.junit.jupiter.engine.descriptor.ClassBasedTestDescriptor.lambda$testInstancesProvider$5(ClassBasedTestDescriptor.java:278)
        at org.junit.jupiter.engine.execution.TestInstancesProvider.getTestInstances(TestInstancesProvider.java:31)
        at org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.lambda$prepare$0(TestMethodTestDescriptor.java:106)
        at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
        at org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.prepare(TestMethodTestDescriptor.java:105)
        at org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.prepare(TestMethodTestDescriptor.java:69)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$prepare$2(NodeTestTask.java:123)
        at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.prepare(NodeTestTask.java:123)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:90)
        at java.base@22.0.2/java.util.ArrayList.forEach(ArrayList.java:1597)
        at org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.invokeAll(SameThreadHierarchicalTestExecutorService.java:41)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:155)
        at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:141)
        at org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:139)
        at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:138)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:95)
        at java.base@22.0.2/java.util.ArrayList.forEach(ArrayList.java:1597)
        at org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.invokeAll(SameThreadHierarchicalTestExecutorService.java:41)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:155)
        at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:141)
        at org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:139)
        at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:138)
        at org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:95)
        at org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.submit(SameThreadHierarchicalTestExecutorService.java:35)
        at org.junit.platform.engine.support.hierarchical.HierarchicalTestExecutor.execute(HierarchicalTestExecutor.java:57)
        at org.junit.platform.engine.support.hierarchical.HierarchicalTestEngine.execute(HierarchicalTestEngine.java:54)
        at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:198)
        at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:169)
        at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:93)
        at org.junit.platform.launcher.core.EngineExecutionOrchestrator.lambda$execute$0(EngineExecutionOrchestrator.java:58)
        at org.junit.platform.launcher.core.EngineExecutionOrchestrator.withInterceptedStreams(EngineExecutionOrchestrator.java:141)
        at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:57)
        at org.junit.platform.launcher.core.DefaultLauncher.execute(DefaultLauncher.java:103)
        at org.junit.platform.launcher.core.DefaultLauncher.execute(DefaultLauncher.java:94)
        at org.junit.platform.launcher.core.DelegatingLauncher.execute(DelegatingLauncher.java:52)
        at org.junit.platform.launcher.core.SessionPerRequestLauncher.execute(SessionPerRequestLauncher.java:70)
        at org.graalvm.junit.platform.NativeImageJUnitLauncher.execute(NativeImageJUnitLauncher.java:74)
        at org.graalvm.junit.platform.NativeImageJUnitLauncher.main(NativeImageJUnitLauncher.java:129)
        at java.base@22.0.2/java.lang.invoke.LambdaForm$DMH/sa346b79c.invokeStaticInit(LambdaForm$DMH)
Caused by: com.oracle.svm.core.jdk.UnsupportedFeatureError: Defining hidden classes at runtime is not supported.
        at org.graalvm.nativeimage.builder/com.oracle.svm.core.util.VMError.unsupportedFeature(VMError.java:121)
        at java.base@22.0.2/java.lang.ClassLoader.defineClass0(ClassLoader.java:323)
        at java.base@22.0.2/java.lang.System$2.defineClass(System.java:2402)
        at java.base@22.0.2/java.lang.invoke.MethodHandles$Lookup$ClassDefiner.defineClass(MethodHandles.java:2492)
        at java.base@22.0.2/java.lang.invoke.InnerClassLambdaMetafactory.generateInnerClass(InnerClassLambdaMetafactory.java:364)
        ... 87 more

2024-08-05T05:18:53.726681Z main ERROR Log4j2 could not find a logging implementation. Please add log4j-core to the classpath. Using SimpleLogger to log to the console...
testhivedrivertable
key     int
value   string
com.lingh.HiveTest > test() SUCCESSFUL


Test run finished after 24163 ms
[         2 containers found      ]
[         0 containers skipped    ]
[         2 containers started    ]
[         0 containers aborted    ]
[         2 containers successful ]
[         0 containers failed     ]
[         1 tests found           ]
[         0 tests skipped         ]
[         1 tests started         ]
[         0 tests aborted         ]
[         1 tests successful      ]
[         0 tests failed          ]

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  03:18 min (Wall Clock)
[INFO] Finished at: 2024-08-05T13:19:17+08:00
[INFO] ------------------------------------------------------------------------

```
