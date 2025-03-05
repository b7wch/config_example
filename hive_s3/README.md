# Issues from Hive and HMS using s3 as filesystem 

## env
HIVE_VERSION=3.1.3
JAR_LIB_DIR= yourlibdirname


## hive
When Hive 3.1.3 uses S3 as the file system, it is necessary to include hadoop-aws 3.1.3 and its related components, which depend on guava-27.0-jre.jar. 

However, Hive 3.1.3 comes with guava-19.0.jar by default. Therefore, guava-19.0.jar needs to be replaced with guava-27.0-jre.jar.

## Tez

Hive 3.1.3 comes with Tez framework version 0.9, which depends on a Guava library version that is incompatible with guava-27.0-jre.jar. Therefore, the Tez version needs to be upgraded to 0.10.1 [(download addr)](https://dlcdn.apache.org/tez/0.10.1/apache-tez-0.10.1-bin.tar.gz).


## Tez AES Authorization Error

Error maybe like:
```
Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: org.apache.hadoop.fs.s3a.AWSClientIOException: doesBucketExist on test: com.amazonaws.AmazonClientException: No AWS Credentials provided by BasicAWSCredentialsProvider EnvironmentVariableCredentialsProvider InstanceProfileCredentialsProvider : com.amazonaws.AmazonServiceException: writing response to 169.254.169.254:80: reading HTTP GET: unexpected EOF (Service: null; Status Code: 500; Error Code: null; Request ID: null): No AWS Credentials provided by BasicAWSCredentialsProvider EnvironmentVariableCredentialsProvider InstanceProfileCredentialsProvider : com.amazonaws.AmazonServiceException: writing response to 169.254.169.254:80: reading HTTP GET: unexpected EOF (Service: null; Status Code: 500; Error Code: null; Request ID: null)
```

Through the way to add config 
```
<property>
    <name>hive.conf.hidden.list</name>
    <value>javax.jdo.option.ConnectionPassword,hive.server2.keystore.password,fs.s3a.proxy.password,dfs.adls.oauth2.credential,fs.adl.oauth2.credential</value>
</property>
```
to hive-site.xml config file, will resolve this problem.
