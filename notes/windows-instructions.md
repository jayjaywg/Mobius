# Building Mobius

## Prerequisites

* Windows Server 2012 or above; or, 64-bit Windows 7 or above.
* Developer Command Prompt for [Visual Studio](https://www.visualstudio.com/) 2013 or above, which comes with .NET Framework 4.5 or above. Note: [Visual Studio 2015 Community Edition](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx) is **FREE**.
* 64-bit JDK 7u85 or above; or, 64-bit JDK 8u60 or above. OpenJDK for Windows can be downloaded from [http://www.azul.com/downloads/zulu/zulu-windows/](http://www.azul.com/downloads/zulu/zulu-windows/); Oracle JDK8 for Windows is available at Oracle website.

JDK should be downloaded manually, and the following environment variables should be set properly in the Developer Command Prompt for Visual Studio:

* `JAVA_HOME`


## Instructions

* In the Developer Command Prompt for Visual Studio where `JAVA_HOME` is set properly, navigate to [Mobius\build](../build/) directory: 

	```  
	Build.cmd  
	```

* Optional: 
	- Under [Mobius\scala](../scala) directory, run the following command to clean spark-clr*.jar built above: 

		```  
		mvn clean
		```  

 	- Under [Mobius\csharp](../csharp) directory, run the following command to clean the .NET binaries built above:

		```  
		Clean.cmd  
		```  
		
[Build.cmd](../build/Build.cmd) downloads necessary build tools; after the build is done, it prepares the folowing directories under `Mobius\build\runtime`:

  * **lib** ( `spark-clr*.jar` )  
  * **bin** ( `Microsoft.Spark.CSharp.Adapter.dll`, `CSharpWorker.exe`)  
  * **samples** ( The contents of `Mobius\csharp\Samples\Microsoft.Spark.CSharp\bin\Release\*`, including `Microsoft.Spark.CSharp.Adapter.dll`, `CSharpWorker.exe`, `SparkCLRSamples.exe`, `SparkCLRSamples.exe.Config` etc. ) 
  * **scripts** ( `sparkclr-submit.cmd` )  
  * **data** ( `Mobius\csharp\Samples\Microsoft.Spark.CSharp\data\*` )    

# Running Samples

## Prerequisites

JDK should be downloaded manually, and the following environment variables should be set properly in the Developer Command Prompt for Visual Studio:

* `JAVA_HOME`

## Running in Local mode

In the Developer Command Prompt for Visual Studio where `JAVA_HOME` is set properly, navigate to [Mobius\build](../build/) directory:

```  
RunSamples.cmd  
```

It is **required** to run [Build.cmd](../build/Build.cmd) prior to running [RunSamples.cmd](../build/RunSamples.cmd).

[RunSamples.cmd](../build/localmode/RunSamples.cmd) downloads the version of Apache Spark referenced in the current branch, sets up `SPARK_HOME` environment variable, points `SPARKCLR_HOME` to `Mobius\build\runtime` directory created by [Build.cmd](../build/Build.cmd), and invokes [sparkclr-submit.cmd](../scripts/sparkclr-submit.cmd), with `spark.local.dir` set to `Mobius\build\runtime\Temp`.

A few more [RunSamples.cmd](../build/localmode/RunSamples.cmd) examples:
- To display all options supported by [RunSamples.cmd](../build/localmode/RunSamples.cmd): 

    ```  
    RunSamples.cmd  --help
    ```

- To run PiSample only:

    ```  
    RunSamples.cmd  --torun pi*
    ```

- To run PiSample in verbose mode, with all logs displayed at console:

    ```  
    RunSamples.cmd  --torun pi* --verbose
    ```

## Running in Standalone mode

```
sparkclr-submit.cmd --verbose --master spark://host:port --exe SparkCLRSamples.exe  %SPARKCLR_HOME%\samples sparkclr.sampledata.loc hdfs://path/to/mobius/sampledata
```
- When option `--deploy-mode` is specified with `cluster`, option `--remote-sparkclr-jar` is required and needs to be specified with a valid file path of spark-clr*.jar on HDFS.

## Running in YARN mode

```
sparkclr-submit.cmd --verbose --master yarn-cluster --exe SparkCLRSamples.exe %SPARKCLR_HOME%\samples sparkclr.sampledata.loc hdfs://path/to/mobius/sampledata
```

# Running Unit Tests

* In Visual Studio: Install NUnit3 Test Adapter. Run the tests through "Test" -> "Run" -> "All Tests"

* Install NUnit Runner 3.0 or above using NuGet (see [https://www.nuget.org/packages/NUnit.Runners/](https://www.nuget.org/packages/NUnit.Runners/)). In Developer Command Prompt for VS, set `NUNITCONSOLE` to the path to nunit console, and navigate to `Mobius\csharp` and run the following command: 
    ```
    Test.cmd
    ```
