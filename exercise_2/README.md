![WTT logo](../assets/wtt-logo.png)

# Automated Exploratory Testing Workshop - Exercise 2

In this exercise you will learn about managing dynamic components on websites using:

* [Hide Modifier](https://github.com/wttech/aet/wiki/HideModifier),
* [Resolution Modifier](https://github.com/wttech/aet/wiki/ResolutionModifier),
* [Sleep Modifier](https://github.com/wttech/aet/wiki/SleepModifier),
* and [Wait For Page Loaded Modifier](https://github.com/wttech/aet/wiki/WaitForPageLoadedModifier).

Please see `exercise2-explained.xml` for a detailed explanation of the structure of the suite XML file.

## Running suite
In order to run the suite on your local Vagrant machine, run the following command in this directory:

`mvn aet:run -DtestSuite=exercise2.xml -Ddomain=http://zg.cognifide.com/aet/testWarez2016`

This action will execute the `exercise2` suite.
You can learn more about running the suite at [AET wiki](https://github.com/wttech/aet/wiki/RunningSuite) and [Workshop description](https://github.com/Skejven/aet-workshop#running-suite).

Please notice, that the parameter `domain` can also be defined in the suite XML file (see `exercise1-explained.xml` for a detailed explanation).
When the domain is specified in XML, you may run AET with no `-Ddomain=http://zg.cognifide.com/aet/testWarez2016` parameter (if you do so, it will override the one defined in the suite).  

## Exercise
In this exercise you will be modifying the `exercise2.xml` suite to prepare the suite that will test pages with dynamic components.

#### 1. Hide dynamic and frequently changing components
Update the `hide-dynamic-elements` test from `exercise2.xml` to obtain a stable report that will present differences between `about.html` page *screenshots* in the 1200x800 resolution.

[Run suite](#running-suite) and check your report. You will see that the test with dynamic elements failed because of dynamic components rendered on the page:

* moving advertisement,
* tweets feed.

Perform the following steps:
   * [hide](https://github.com/wttech/aet/wiki/HideModifier) the moving advertisement using the `xpath` parameter - it can be obtained e.g. by inspecting the element and copying its xpath in Chrome,
   * [hide](https://github.com/wttech/aet/wiki/HideModifier) the tweets feed using the `xpath` parameter.

[Run suite](#running-suite) and check your report.

You will notice that the collected screenshot (the right one - `View`) has no dynamic elements. Accept the url and [run suite](#running-suite) again.

------

#### 2. Capture a screenshot in different resolutions (check page responsiveness)
Update the `hide-dynamic-elements` test from `exercise2.xml` to collect and compare three screenshots in the following resolutions:

* 1200 x 800 px for `desktop` screen size simulation,
* 768 x 1024 px for `tablet` screen size simulation,
* 320 x 568 px for `mobile` screen size simulation.

Perform the following steps:
   * modify the view [resolution](https://github.com/wttech/aet/wiki/ResolutionModifier) after existing `desktop` screenshot capture, set it to 768 x 1024,
   * it is a good practice to let the page adjust to a new resolution before capturing a screenshot, wait 1000 ms (1 second) using [sleep modifier](https://github.com/wttech/aet/wiki/SleepModifier),
   * add [screen collector](https://github.com/wttech/aet/wiki/ScreenCollector) to capture a page screenshot and name it `tablet`,
   * perform similar steps for the `mobile` resolution,
   * notice you don't need to call [layout comparator](https://github.com/wttech/aet/wiki/LayoutComparator) separately for each captured screenshot - the one existing will take all the collected screenshots.
   
[Run suite](#running-suite) and check your report.

------

#### 3. Testing pages with dynamic js processing
Update the `wait-for-dynamic-element` test from `exercise2.xml` to wait for an animation that finishes long after the page is rendered.

The page `/contact.html` contains the resource that needs at least 5 seconds to load. The test should wait for it before taking a screenshot.

Perform the following steps:
   * use [Sleep Modifier](https://github.com/wttech/aet/wiki/SleepModifier) to wait 6 seconds (6000 ms) before running screen collection.
   
[Run suite](#running-suite) and check your report.