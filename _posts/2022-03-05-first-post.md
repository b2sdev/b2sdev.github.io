---
title: "[Spark] DBConnect의 ProtoSerializer StackOverflowError"
date: 2022-03-16 05:00:00 +0900
categories: blog
tags: spark databricks dbconnect
---

로컬 PC에서 Databricks Connect를 사용하면 로컬에서 개발한 Spark 코드를 Databricks의 Spark 클러스터에 배포하여 원격으로 실행할 수 있다.

## 문제
초기 셋업 이후 Spark 애플리케이션의 실행이 잘 되다가고, 연산 중에 간혹 ProtoSerializer StackOverflowError가 발생하는 경우가 있다. (특히, PySpark로 작성한 코드를 실행 시)

```console
py4j.protocol.Py4JJavaError: An error occurred while calling o945.count.
: java.lang.StackOverflowError
    at java.lang.Class.getEnclosingMethodInfo(Class.java:1072)
    at java.lang.Class.getEnclosingClass(Class.java:1272)
    at java.lang.Class.getSimpleBinaryName(Class.java:1443)
    at java.lang.Class.getSimpleName(Class.java:1309)
    at org.apache.spark.sql.types.DataType.typeName(DataType.scala:67)
    at org.apache.spark.sql.types.DataType.simpleString(DataType.scala:82)
    at org.apache.spark.sql.types.DataType.sql(DataType.scala:90)
    at org.apache.spark.sql.util.ProtoSerializer.serializeDataType(ProtoSerializer.scala:3207)
    at org.apache.spark.sql.util.ProtoSerializer.serializeAttrRef(ProtoSerializer.scala:3610)
    at org.apache.spark.sql.util.ProtoSerializer.serializeAttr(ProtoSerializer.scala:3600)
    at org.apache.spark.sql.util.ProtoSerializer.serializeNamedExpr(ProtoSerializer.scala:3537)
    at org.apache.spark.sql.util.ProtoSerializer.serializeExpr(ProtoSerializer.scala:2323)
    at org.apache.spark.sql.util.ProtoSerializer$$anonfun$$nestedInanonfun$serializeCanonicalizable$1$1.applyOrElse(ProtoSerializer.scala:3001)
    at org.apache.spark.sql.util.ProtoSerializer$$anonfun$$nestedInanonfun$serializeCanonicalizable$1$1.applyOrElse(ProtoSerializer.scala:2998)
```
 
## 원인
원인은 로컬 PC에서 충분한 메모리가 할당되지 않았기 때문이다.
 
DBConnect로 배포하는 Spark 코드 전부가 런타임에 Databricks의 Spark 클러스터에서 실행되는 것이 아니라, 일부 코드는 클라이언트 PC에서 로컬로 처리된다. 즉, 드라이버는 로컬 PC의 JVM 프로세스에서 실행된다.

<figure>
  <img src='https://freecontent.manning.com/wp-content/uploads/bonaci_runtimeArch_01.png' alt='클러스터 배포 모드'>
  <figcaption>[기대했던 클러스터 배포 모드]</figcaption>
</figure>

<figure>
  <img src='https://freecontent.manning.com/wp-content/uploads/bonaci_runtimeArch_02.png' alt='클라이언트 배포 모드'>
  <figcaption>[실제로는 클라이언트 배포 모드로 동작함]</figcaption>
</figure>

## 해결
본 문제를 해결하기 위해서는 로컬 PC에서 Apache Spark 드라이버에 할당된 메모리를 늘려야 한다.
 
spark-defaults.conf 파일에 아래 설정을 추가하여 메모리를 늘릴 수 있다(설정 파일은 ${spark_home}/conf/ 폴더에 있으며 없으면 생성한다).
 
```text
spark.driver.memory 4g
spark.driver.extraJavaOptions -Xss32M
```

참고로, 위의 설정을 드라이버 프로세스를 실행할 때 SparkConf에 직접 명시하는 방식으로는 해결이 되지 않는다. Spark Web UI에서 configuration이 정상적으로 설정되어 있는 것을 확인할 수 있어도.
 
```python
spark = SparkSession.builder.config("spark.driver.memory", "4g").getOrCreate()
```

드라이버 프로세스를 띄웠다는 것 자체는 이미 드라이버 JVM이 시작되었음을 의미한다. 따라서, 이후에 드라이버의 configuration을 바꾸어봤자 적용이 될리 만무하다. 스파크 문서에서는 아래와 같이 안내하고 있다.

> Note: In client mode, this config must not be set through the SparkConf directly in your application, because the driver JVM has already started at that point. Instead, please set this through the --driver-memory command line option or in your default properties file.

## 결론
* Databricks Connect로 로컬 PC에서 작성한 Spark 코드를 Databricks의 Spark 클러스터에 배포하여 실행 시, 드라이버 프로그램은 로컬 PC의 JVM 프로세스에서 실행된다.
* StackoverflowError가 발생하면 로컬 PC의 메모리를 늘려야 한다.

## 참고
* https://docs.microsoft.com/ko-kr/azure/databricks/dev-tools/databricks-connect
* https://docs.microsoft.com/ko-kr/azure/databricks/kb/dev-tools/dbconnect-protoserializer-stackoverflow
* https://stackoverflow.com/questions/53606756/how-to-set-spark-driver-memory-in-client-mode-pyspark-version-2-3-1
* Petar Zecevic, Marko Bonaci, <<Spark in Action>>
