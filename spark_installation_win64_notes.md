
## Apache Spark & Jupyter on Windows

Below steps are necessary to setup Spark on a Windows 64-bit machine for use with Python and Jupyter notebooks. **Note**: This assumes a pre-existing Anaconda installation for Python 3.5+ along with Unix utilities.  

* Follow instructions on this [blog](https://medium.com/@GalarnykMichael/install-spark-on-windows-pyspark-4498a5d8d66c) to install Spark and PySpark. **Note**: This note assumes a spark install under `C:\`
* Currently Spark only supports Java 8 (i.e.: JDK 1.8). If you have an installation of Java 9 or 10 you will need to revert back to Java 8. Use [this site](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) for Java 8 downloads. **Note**: There may be "path not found" issues when Java is installed under `C:\Program Files` or `C:\Program Files(x86)`. It is best to install under `C:\Java\jdk1.8.x_xxx`.
* Set the following environment variables using the control panel: System->Advance System Settings->Environment Variables  
  <code>
  SPARK_HOME=C:\spark-2.x.x-bin-hadoop2.7  
  HADOOP_HOME=C:\spark-2.x.x-bin-hadoop2.7  
  JAVA_HOME=C:\Java\jdk1.8.x_xxx\jre  
  PATH=%PATH%;C:\spark-2.x.x-bin-hadoop2.7\bin  
  PYSPARK_DRIVER_PYTHON=jupyter  
  PYSPARK_DRIVER_PYTHON_OPTS=notebook  
  </code>  
* Clone your current Python 3 virtual env for Spark using conda.  
  **Note**: this is useful since pyspark version dependencies usually lag behind latest versions of packages. e.g.: sklearn  
  <code>
  conda create --name ipykernel_py3_spark --clone ipykernel_py3  
  </code>
* Install `pyspark` on the spark specific environment  
  <code>
  activate ipykernel_py3_spark  
  pip install pyspark  
  </code>  
* Install `findspark`. **Note**: this helps locate `pyspark` for use within python when `pyspark` is not part of `sys.path`  
  <code>
  pip install findspark
  </code>  
* Now starting `pyspark` should open up a Jupyter notebook. Here we are starting Spark with two local nodes.  
  <code>
  pyspark --master local[2]
  </code>  
* Add the following to the Jupyter notebook:  
  <code>
  import findspark  
  findspark.init()  
  </code>  
  <code>
  from pyspark import SparkConf, SparkContext  
  from pyspark.sql import SparkSession, SQLContext  
  </code>  
  <code>
  spark = SparkSession.builder.getOrCreate()  
  df = spark.sql('''select 'spark' as hello ''')  
  df.show()  
  </code>

  This should result in the below output:  

  +-----+  
  |hello|  
  +-----+  
  |spark|  
  +-----+  
