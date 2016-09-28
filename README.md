![Cognifide logo](http://cognifide.github.io/images/cognifide-logo.png)

# Automated Exploratory Testing Workshop
<p align="center">
  <a href="https://github.com/Cognifide/aet" target="_blank">
    <img src="assets/aet-logo-black.png" alt="AET Logo"/>
  </a>
</p>

## AET setup

### 1. Please make sure that you have the following software installed on your local machine (versions are very important):
   * [VirtualBox 5.0.26](https://www.virtualbox.org/wiki/Download_Old_Builds_5_0)
      * To check its version, open VirtualBox installed on your computer. Navigate to `Help` and `About VirtualBox...`, the version of the installed software should be visible at the bottom of the popup.
   * [Vagrant 1.8.4](https://releases.hashicorp.com/vagrant/1.8.4/)
      * To check its version, open the command line console and execute `vagrant -v`.
   * [ChefDK 0.17.17](https://downloads.chef.io/chef-dk/)
      * To check its version, open the command console and execute `chef -v`. After `Chef Development Kit Version` the version of ChefDK should be displayed.
   * [Maven](https://maven.apache.org/download.cgi) (at least version 3.0.4)
      * To check its version, open the command console and execute `mvn -v`. Please make also sure, that `Java home` and `Maven home` properties are set.
   * [JDK 7 or 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)
      * To check its version, open the command console and execute `java -version`.
   * [Chrome browser](https://www.google.com/chrome/browser/desktop/)

### 2. Download AET Vagrant
Please navigate to [AET GitHub Repository](https://github.com/Cognifide/aet) and download or clone it using Git.

![Get vagrant](assets/get-vagrant.png)

   * to download the repo, simply click `Download ZIP` and unpack it to your workspace directory,
   * to clone the repository use your favourite Git client with the following repository address `https://github.com/Cognifide/aet.git` (or `git@github.com:Cognifide/aet.git` if you have a GitHub account).

### 3. Start AET Vagrant machine
Please navigate to the `vagrant` directory in your local AET repository.
Open the command prompt as the Administrator and execute the following commands:

   * `vagrant plugin install vagrant-omnibus`
   * `vagrant plugin install vagrant-berkshelf`
   * `vagrant plugin install vagrant-hostmanager`

Run `berks install` and then `vagrant up` to start a virtual machine. This process may take a few minutes.

### 4. Check your machine
Enter [http://aet-vagrant:8181/system/console](http://aet-vagrant:8181/system/console) to check setup status after vagrant finishes AET setup.

Console credentials are: 

   * username `karaf`, 
   * password `karaf`.

You should see the information `Bundle information: 251 bundles in total - all 251 bundles active`.

#### Running suite

Now you are ready to run your first suite to check if the instance is running properly. To do it, create the `aet-test` directory.
Inside the directory create two files `pom.xml` and `suite.xml` with the content defined below:

**pom.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.cognifide.aet.workshop</groupId>
  <artifactId>my-project</artifactId>
  <version>0.1.0</version>
  <packaging>pom</packaging>

  <name>AET :: Test Suite Execution</name>
  <url>https://github.com/Cognifide/aet</url>

  <properties>
    <aet.version>2.0.0</aet.version>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <build>
    <plugins>
      <plugin>
        <groupId>com.cognifide.aet</groupId>
        <artifactId>aet-maven-plugin</artifactId>
        <version>${aet.version}</version>
      </plugin>
    </plugins>
  </build>

</project>

```

**suite.xml**
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<suite name="YOUR_INITIALS_HERE_LOWERCASE" company="YOUR_COMPANY_NAME_HERE_LOWERCASE" project="workshops">
    <test name="my-first-test">
        <collect>
            <open/>
            <wait-for-page-loaded/>
            <resolution width="1200" height="800"/>
            <sleep duration="2000"/>
            <screen name="desktop"/>
        </collect>
        <compare>
            <screen comparator="layout"/>
        </compare>
        <urls>
            <url href="https://www.google.pl/"/>
        </urls>
    </test>
</suite>
```

Run it executing the following maven command inside the `aet-test` directory using the command line:

`mvn aet:run`

This action will execute the `check` suite. 
You can learn more about running suites in [AET wiki](https://github.com/Cognifide/aet/wiki/RunningSuite).

While running the suite you should see its progress in the console. It may look as follows:

```
[INFO] ********************************************************************************
[INFO] ********************** Job Setup finished at 16:13:24.388.**********************
[INFO] *** Suite is now processed by the system, progress will be available below. ****
[INFO] ********************************************************************************
[INFO] [16:13:24.465]: COLLECTED: [success: 0, total: 1] ::: COMPARED: [success: 0, total: 0]
[INFO] [16:13:30.447]: COLLECTED: [success: 1, total: 1] ::: COMPARED: [success: 1, total: 1]
```

After suite processing is finished you should see the following information with a url to the report:

```
[INFO] Received report message: FinishedSuiteProcessingMessage{correlationId=cognifide-workshops-check-1474577335419, status=OK, errors=[]}
[INFO] Report is available at http://aet-vagrant/report.html?company=cognifide&project=workshops&correlationId=cognifide-workshops-check-1474577335419
[INFO] Suite processing finished.
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 8.721s
[INFO] Finished at: Thu Sep 22 16:13:30 CEST 2016
[INFO] Final Memory: 11M/204M
[INFO] ------------------------------------------------------------------------
```

After you navigate to the report url: 
`http://aet-vagrant/report.html?company=cognifide&project=workshops&correlationId=cognifide-workshops-check-1474577335419`
you will see the report. Another way to open the report is to open `redirect.html` from the `target` directory file that is the result of a Maven build. 
It will redirect the browser to the report web application. 

If you can see the report with a screenshot captured it means you are ready to start your AET adventure.

## Exercises
There are 3 simple exercises that will quickly introduce you to the AET World. 
Order of performing exercises is not important, however we suggest starting with `exercise 1`.
Before running each exercise suite check out the `explained` version of the suite which contains comments with suite details.

You will find each exercise description inside its directory.

You will also find in this repository `pom-template.xml` and `suite-template.xml` that may be useful in the future to setup your own tests.

The directory `exercise-page` contains sources of the page that is used in the workshop. You may simply run it by using [Apache Server](https://httpd.apache.org/download.cgi).