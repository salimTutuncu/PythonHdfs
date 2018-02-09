# PythonHdfs

Python has some WebHDFS interfaces like:

1. pywebhdfs
2. hdfscli

So if we have some trivial task, or if we just want to test something we can choose one of the WebHDFS above. For my opinion hdfscli is the best option for this kind of things. 

Some of the features:

* Python (2 and 3) bindings for the WebHDFS (and HttpFS) API, supporting both secure and insecure clusters.
* Command line interface to transfer files and start an interactive client shell, with aliases for convenient namenode URL caching.
* Additional functionality through optional extensions: avro, to read and write Avro files directly from HDFS. dataframe, to load and save Pandas dataframes. kerberos, to support Kerberos authenticated clusters.

For light load applications, WebHDFS and native protobuf RPCs provide comparable data throughput, but native connectivity is generally considered to be more scalable and suitable for production use.

## Native RPC access in Python

### 1. libhdfs 
The "official" way in Apache Hadoop to connect natively to HDFS from a C-friendly language like Python is to use.

#### Advantages
* A primary benefit of libhdfs is that it is distributed and supported by major Hadoop vendors, and it's a part of the Apache Hadoop project.
* libhdfs, despite being Java and JNI-based, achieves the best throughput in this tests.

#### Disadvantages
* A downside is that it uses JNI (spawning a JVM within a Python process) and requires a complete Hadoop Java distribution on the client side. (To use libhdfs, users must deploy the HDFS jars on every machine.)

#### Python Interfaces
-cyhdfs (using Cython), libpyhdfs (plain Python C extension), and pyhdfs (using SWIG).


### 2. libhdfs3 
It's now part of Apache HAWQ (incubating), a pure C++ library developed by Pivotal Labs for use in the HAWQ SQL-on-Hadoop system.

#### Advantages:
* Light-weight
* It gets rid of the drawbacks of JNI
* it is easy to use and deploy.

#### Disadvantages:
* libhdfs3 performs poorly in small size reads.

#### Python Interfaces
hdfs3, a pure Python interface to libhdfs3 that uses ctypes to avoid C extensions. It provides a Python file interface and access to the rest of the libhdfs3 functionality:


## Conclusion
* Chosing the right python library is highly depends our requirement. If we want to test some small thing then we can go for  hdfscli, if we are looking something for production use, then we have 2 different options; libhdfs and libhdfs3. Both of them has their own advantages and disadvantages. libhdfs3 is more light-weight and easy to use, on the otherhand libhdfs throughput is better than libhdfs3, so for example if we don't need very high throughput for our application we can choose libhdfs3 otherwise, libhdfs would be better option. 

**Note:** Apart from those libraries mentioned above there is also another nice library called *Snakebite* but since Snakebite supports only download it's not listed in. (copyFromLocal not implemented) 
(see https://github.com/spotify/snakebite/issues/37)
