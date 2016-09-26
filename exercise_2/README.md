![Cognifide logo](http://cognifide.github.io/images/cognifide-logo.png)

# Automated Exploratory Testing Workshop - Exercise 2

In this exercise you will learn about dealing with dynamic components on websites using:

* [Hide Modifier](https://github.com/Cognifide/aet/wiki/HideModifier),
* [Resolution Modifier](https://github.com/Cognifide/aet/wiki/ResolutionModifier),
* [Sleep Modifier](https://github.com/Cognifide/aet/wiki/SleepModifier),
* and [Wait For Page Loaded Modifier](https://github.com/Cognifide/aet/wiki/WaitForPageLoadedModifier).

Please see `exercise2-explained.xml` for detailed explanation of suite XML file structure.

## Running suite
In order to run suite on your local Vagrant machine, run following command in this directory:

`mvn aet:run -DtestSuite=exercise2.xml -Ddomain=DOMAIN_WHERE_EXERCISE_PAGE_IS_HOSTED`

This action will execute `exercise2` suite. 
Please remember about changing `DOMAIN_WHERE_EXERCISE_PAGE_IS_HOSTED` to the real domain where the exercise page is hosted.
You can learn more about running suite in [AET wiki](https://github.com/Cognifide/aet/wiki/RunningSuite) and [Workshop description](https://github.com/Skejven/aet-workshop#running-suite).

## Exercise
In this exercise you will be modifying `exercise2.xml` suite to prepare suite that will test pages with dynamic components.

#### 1. Hide dynamic and frequently changing components
Update `hide-dynamic-elements` test from `exercise2.xml` to obtain a stable report that will present differences between `about.html` page *screenshots* in 1200x800 resolution.

[Run suite](#running-suite) and check your report. You will see that test with dynamic elements failed because of dynamic components rendered on a page:

* moving advertisement,
* tweets feed.

Perform following steps:
   * [hide](https://github.com/Cognifide/aet/wiki/HideModifier) moving advertisement using `xpath` parameter - it can be obtained e.g. by inspecting element and copying its xpath in Chrome,
   * [hide](https://github.com/Cognifide/aet/wiki/HideModifier) tweets feed using `xpath` parameter.

[Run suite](#running-suite) and check your report.

You will notice that collected screenshot (the right one - `View`) has no dynamic elements. Accept url and [run suite](#running-suite) again.

------

#### 2. Capture screenshot in different resolutions (check page responsiveness)
Update `hide-dynamic-elements` test from `exercise2.xml` to collect and compare three screenshots in resolutions:

* 1200 x 800 px for `desktop` screen size simulation,
* 768 x 1024 px for `tablet` screen size simulation,
* 320 x 568 px for `mobile` screen size simulation.

Perform following steps:
   * modify view [resolution](https://github.com/Cognifide/aet/wiki/ResolutionModifier) after existing `desktop` screenshot capture, set it to 768 x 1024,
   * it is a good practice to let the page adjust to the new resolution before capturing a screenshot, wait 1000 ms (1 second) using a [sleep modifier](https://github.com/Cognifide/aet/wiki/SleepModifier),
   * add [screen collector](https://github.com/Cognifide/aet/wiki/ScreenCollector) to capture page screenshot and name it `tablet`,
   * perform similar steps for `mobile` resolution,
   * notice you don't need to call [layout comparator](https://github.com/Cognifide/aet/wiki/LayoutComparator) separately for each captured screenshot - the one existing will take of all collected screenshot.
   
[Run suite](#running-suite) and check your report.

------

#### 3. Testing pages with dynamic js processing
Update `wait-for-dynamic-element` test from `exercise2.xml` to wait for animation that finishes long after page is rendered.

Page `/contact.html` contains resource that need at least 5 seconds to load. The test should wait for it before taking a screenshot.

Perform following steps:
   * use [Sleep Modifier](https://github.com/Cognifide/aet/wiki/SleepModifier) to wait 6 seconds (6000 ms) before running screen collection.
   
[Run suite](#running-suite) and check your report.