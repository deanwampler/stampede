# Stampede FAQs

*Copyright (c) 2011-2013, Think Big Analytics, Inc. All Rights Reserved.*

## [Idioms and Patterns][idioms_patterns]
* [Hadoop jobs launch asynchronously. How can I trigger another process when they are finished?][async_hadoop]

## [Customization][customization]
* [How do I customize the behavior of *Stampede*?][customize]

## [Miscellaneous][misc]
* [How do I customize the behavior of *Stampede*?][customize]
* [How can I contribute back to *Stampede*?][contribute]
* [Why does *Stampede* support Mac OSX, as well as Linux?][OSX]

## Idioms and Patterns [idioms_patterns]

### Hadoop jobs launch asynchronously. How can I trigger another process when they are finished? [async_hadoop]

Usually, a process like a Hadoop job will write some final output to a file. Use `try-until` or `try-for` to watch for the output to be created. However, be careful of the case where output is in the process of being written, but not yet finished. For this reason, Hadoop MapReduce jobs write a zero-length marker file named `_SUCCESS` after all job output is finished. It goes into the same directory as the actual output.

So, suppose a big MapReduce job is writing to `/user/me/myjob` in HDFS, a subsequent `make` target and build rules could look like the following, where it tests for the existence of the `_SUCCESS` file for up to four hours, checking every ten minutes:

    postprocess:
      @try-for 4h 10m "hadoop fs -test -e /user/me/myjob/_SUCCESS"
      postprocess_the_data

If you know the job id, an alternative test condition is to examine the output of one of the `hadoop job -xyz` commands.

## Customization [customization]

### How do I customize the behavior of *Stampede*? [customize]

* Pass arguments to `$STAMPEDE_HOME/bin/stampede`. See `stampede --help` for details.
* Override an environment variable in a custom `.stampederc` file. See the [README](README.html) for details.
* Create your own version of an existing `bin` script or a new script, and put it in `$STAMPEDE_HOME/custom`. For example, you can change the way log messages are formatted by creating your own `format-log-message` script.

## Miscellaneous [misc]

### How can I contribute back to *Stampede*? [contribute]

Patches are welcome, of course. For larger contributions, we intend the `$STAMPEDE_HOME/contrib` directory to be location for contributions that are "as-is" and not appropriate to roll into the main code base.

We also welcome contributions that support specific tool suites, in addition to Hadoop.

Also, if you have *stampedes* (worflows) that demonstrate useful techniques *or* pain-points to improve, send them our way.

Finally, we welcome patches that enable Stampede to work with Cygwin on Windows and Unix distributions where `bash` and Gnu `make` are installed.

### Why does *Stampede* support Mac OSX, as well as Linux? [OSX]

Like many other server-side applications, *Stampede* was developed on a Mac for use primarily on Linux systems. In principle, it should work with any Unix variant, including Cygwin for Windows, although we have only tested on Mac OSX and several flavors of Linux. See the [README](README.html) for more information on platform issues.
