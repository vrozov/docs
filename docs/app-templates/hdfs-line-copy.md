# HDFS to HDFS line copy application

## Summary

Ingest and backup hadoop HDFS data as lines from one cluster to another in a fault tolerant way. This application reads lines from the configured source path and writes them to the destination file path. The source code is available at: [https://github.com/DataTorrent/app-templates/tree/master/hdfs-line-copy](https://github.com/DataTorrent/app-templates/tree/master/hdfs-line-copy).

Please send feedback or feature requests to: [feedback@datatorrent.com](mailto:feedback@datatorrent.com)

This document has a step-by-step guide to configure, customize, and launch this application.

## <a name="steps_to_launch"></a>Steps to launch application

1. Click on the AppHub tab from the top navigation bar.
   ![AppHub link from top navigation bar](images/common/apphub_link.png)

1. Page listing the applications available on AppHub is displayed.
Search for HDFS to see all applications related to HDFS.
   ![AppHub search for HDFS](images/hdfs-line-copy/apphub-search.png)

   Click on import button for `HDFS to HDFS Line Copy App`

1. Notification is displayed on the top right corner after application package is successfully
   imported.
   ![App import Notification](images/hdfs-line-copy/import-notification.png)

1. Click on the link in the notification which navigates to the page for this application package.
   ![App details page](images/hdfs-line-copy/app-details-page.png)

    Detailed information about the application package like version, last modified time, and short description is available on this page. Click on launch button for `HDFS line copy`
   application.

1. <a name="launch-dialogue"></a>`Launch HDFS-line-copy` dialogue is displayed. One can configure name of this instance of the application after from this dialogue.
   ![Launch dialogue](images/hdfs-line-copy/launch.png)

1. Select `Use saved configuration` option. This displays list of pre-saved configurations.
Please select `sandbox-memory-conf.xml` or `cluster-memory-conf.xml` depending on whether
your environment is the DataTorrent sandbox, or other cluster.
   ![Select saved configuration](images/hdfs-line-copy/saved-conf.png)

1. Select `Specify custom properties` option. Click on `add default properties` button.
   ![Specify custom properties](images/hdfs-line-copy/specify-custom.png)

1. This expands a key-value editor pre-populated with mandatory properties for this application. Change values as needed.
   ![Properties editor](images/hdfs-line-copy/property-editor.png)

    <a name="property-editor"></a>For example, suppose we wish to process lines from all the files in `/user/appuser/input` from `source-cluster` and send the output to `output.txt` in `/user/appuser/output` in the `destination-cluster`. Properties should be set as follows:

    |name|value|
    |-|-|
    |dt.operator.fileOutput.prop.filePath|hdfs://destination-cluster/user/appuser/output|
    |dt.operator.fileOutput.prop.outputFileName|output.txt|
    |dt.operator.recordReader.prop.files|hdfs://source-cluster/user/appuser/input|

    Details about configuration options are available in [Configuration options](#configuration_options) section.

1. Click on `Launch` button on bottom right corner to launch the application.
Notification is displayed on the top right corner after application is launched successfully and includes the Application ID which can be used to monitor this instance and find its logs.
   ![Application launch notification](images/common/app_launch_notification.png)

1. Click on the `Monitor` tab from the top navigation bar.
   ![Monitor tab](images/common/monitor_link.png)

1. Page with listing of all running applications is displayed. Search for current application based on name or application id or any other relevant field. Click on the application name or id to navigate to application instance details page.
   ![Apps monitor listing](images/common/apps_monitor_listing.png)

1. Application instance details page shows key metrics for monitoring the application status. The `logical` tab shows application DAG, Stram events, operator status based on logical operators, stream status, and a chart with key metrics.
   ![Logical tab](images/hdfs-line-copy/logical.png)

1. Click on the `physical` tab to look at the status of physical instances of the operator, containers etc.
   ![Physical tab](images/hdfs-line-copy/physical.png)

## <a name="configuration_options">Configuration options</a>

### Mandatory properties
End user must specify the values for these properties (all properties are strings).

|Property|Description|Example|
|-|-|-|
|<p style="font-size:12px">dt.operator.recordReader.prop.files|HDFS path for input file or directory|<ul><li>/user/appuser/input/directory1</li><li>/user/appuser/input/file2.log</li><li>hdfs://node1.corp1.com/user/user1/input</li></ul>|
|<p style="font-size:12px">dt.operator.fileOutput.prop.outputFileName|Name of the output file. This name will be appended with suffix for each part.|output.txt|
|<p style="font-size:12px">dt.operator.fileOutput.prop.filePath|HDFS path for the output directory. Generally, this refers to path on the hadoop cluster on which app is running.|/user/appuser/output|

### Advanced properties
There are pre-saved configurations based on the application environment. Recommended settings for [datatorrent sandbox edition](https://www.datatorrent.com/download/datatorrent-rts-sandbox-edition-download/) are in `sandbox-memory-conf.xml` and for a cluster environment in `cluster-memory-conf.xml`.

|Property|Description|Type|Default for<br/> cluster<br/>-memory<br/>-conf.xml|Default for<br/>sandbox<br/>-memory<br/>-conf.xml
|-|-|-|-|-|
|dt.operator.recordReader.prop.minReaders|Minimum number of BlockReader partitions for parallel reading.|int|1|1|
|dt.operator.recordReader.prop.maxReaders|Maximum number of BlockReader partitions for parallel reading.|int|16|1|

You can override default values for advanced properties by specifying custom values for these properties in the step [specify custom property](#property-editor) step mentioned in [steps](#steps_to_launch) to launch an application.

## Steps to customize the application

1. Make sure you have following utilities installed on your machine and available on `PATH` in environment variables
    - [Java](https://www.java.com/en/download/manual.jsp) : 1.7.x
    - [maven](http://maven.apache.org/download.cgi) : 3.0 +
    - [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) : 1.7 +
    - [Hadoop]( http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/) (Apache-2.2)+

1.  Use following command to clone the examples repository:

    ```
    git clone git@github.com:DataTorrent/app-templates.git
    ```

1. Change directory to `examples/tutorials/hdfs-to-kafka-sync`:

    ```
    cd examples/tutorials/hdfs-line-copy
    ```

1. Import this maven project in your favorite IDE (e.g. eclipse).

1. Change the source code as per your requirements. Some tips are given as commented blocks in the `Application.java` for this project.

1. Make respective changes in the test case and `properties.xml` based on your environment.

1. Compile this project using maven:

    ```
    mvn clean package
    ```

    This will generate the application package with the `.apa` extension inside the `target` directory.

1. Go to DataTorrent UI Management console on web browser. Click on the `Develop` tab from the top navigation bar.
   ![Develop tab](images/common/develop_link.png)

1. Click on `upload package` button and upload the generated `.apa` file.
   ![Upload](images/common/upload.png)

1. Application package page is shown with the listing of all packages. Click on the `Launch` button for the uploaded application package. Follow the [steps](#launch-dialogue) for launching an application.
