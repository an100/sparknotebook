
# sparknotebook

This project contains samples of ipython notebooks running [Spark](https://spark.apache.org/). One notebook, _2-Text-Analytics.ipynb_ is written in python. The second, _Scala-2-Text-Analytics.ipynb_ is in Scala. The dataset and the most excellent _2-Text-Analytics.ipynb_ are originally from [https://github.com/xsankar/cloaked-ironman](https://github.com/xsankar/cloaked-ironman).

Just open each notebook to see how Spark is instantiated and used.

![Notebook](https://pbs.twimg.com/media/B1O87jsCQAAeLfz.png:large)

## Python Set-up

To run the python notebook, you will need to:

1. Install ipython and ipython notebook. For simplicity, I am just using the free [Anaconda python distribution from Continuum Analytics](http://continuum.io/downloads).
2. [Download and install Spark distribution](https://spark.apache.org/downloads.html). The download includes the `pyspark` script that you need to launch python with Spark.

For best results, cd into this projects root directory before starting ipython. The actual command to start the ipython notebook is:

```bash
IPYTHON=1 IPYTHON_OPTS="notebook --pylab inline" pyspark
```

__NOTE__: Sometimes when running Spark on Java 7 you may get a java.net.UnknownHostException. I have not yet seen this on Java 8. If this happens to you, you can resolve it by setting the __SPARK_LOCAL_IP__ environment variable to `127.0.0.1` before launching Spark. For example:

```bash
SPARK_LOCAL_IP=127.0.0.1 IPYTHON=1 IPYTHON_OPTS="notebook --pylab inline" pyspark
```

## Scala Set-up

To run the scala notebook, you will need to:

1. Create a Scala profile for ipython
    ```
    ipython profile create scala
    ```
    The output from this command will tell you the location of the _ipython_config.py_ file. You will need to edit that file soon.  
2. Download [IScala.jar](https://github.com/mattpap/IScala/releases). You will need to stash it somewhere. I put it in `~/.ipython/profile_scala/lib`
3. Edit your _ipython_config.py_ to tell ipython about IScala
    ```python
    c = get_config()
    
    c.KernelManager.kernel_cmd = ["java", "-jar",
                              "/User/yournamehere/.ipython/profile_scala/lib/IScala.jar",
                              "--profile",
                              "{connection_file}",
                              "--parent"]
    ```

At this point you can start up IScala
```bash
ipython notebook --profile scala
```

If you are running your notebook and it crashes with OutOfMemoryErrors you can increase the amount of memory used with the `-Xmx` flag (e.g. -Xmx2g or -Xmx2048m will both allocate 2GB of memory for the JVM to use):
```bash
SBT_OPTS=-Xmx2048m ipython notebook --profile scala
```

As with the python example, if you get a java.net.UnknownHostException when starting ipython use the following command:

```bash
SPARK_LOCAL_IP=127.0.0.1 SBT_OPTS=-Xmx2048m ipython notebook --profile scala
```

__NOTE:__ For the Scala notebook, you do __not__ need to download and install Spark. The Spark dependencies are managed via sbt which is running under the hood in the Spark notebook.

### Plotting

As of Late-October 2014, IScala has added ploting and rich text output. if you build IScala from source you can have these in your notebooks. There's a [Display.ipynb](https://github.com/mattpap/IScala/blob/master/examples/Display.ipynb) in the IScala project that demonstrates this. Just a note on biulding IScala from source for Spark using SBT:

- IScala cross builds against Scala 2.10 and 2.11. Spark is currently on Scala 2.10. To build the correct IScala jar run: `sbt + assembly`. The correct IScala jar will then be in _IScala/target/scala-2.10/lib/IScala.jar_.
