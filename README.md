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
cd ../

git clone git@github.com:linghengqian/log4j2-v2231-graalvm-native-image-test.git
cd ./log4j2-v2231-graalvm-native-image-test/
sdk use java 22.0.2-graalce
./mvnw clean test
```

- Log as follows.

```shell
$ ./mvnw clean test

```
