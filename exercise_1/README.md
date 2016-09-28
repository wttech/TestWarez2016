![Cognifide logo](http://cognifide.github.io/images/cognifide-logo.png)

# Automated Exploratory Testing Workshop - Exercise 1

In this exercise you will learn about collecting, comparing and understanding the AET report on:

* screenshots,
* js errors,
* source,
* w3c compliance,
* status codes,
* accessibility.

Please see `exercise1-explained.xml` for a detailed explanation of a structure of the suite XML file.

## Running suite
In order to run the suite on your local Vagrant machine, run the following command in this directory:

`mvn aet:run -DtestSuite=exercise1.xml -Ddomain=http://zg.cognifide.com/aet/testWarez2016 -DendpointDomain=http://139.59.210.108:8181`

This action will execute the `exercise1` suite.

If you want to run the suite now, please use `exercise1-explained.xml` - a suite definition in `exercise1.xml` is not ready to perform the test before starting exercises.
You can learn more about running the suite at [AET wiki](https://github.com/Cognifide/aet/wiki/RunningSuite) and [Workshop description](https://github.com/Skejven/aet-workshop#running-suite).

Please notice, that the parameter `domain` can also be defined in the suite XML file (see `exercise1-explained.xml` for a detailed explanation).
When the domain is specified in XML, you may run AET with no `-Ddomain=http://zg.cognifide.com/aet/testWarez2016` parameter (if you do so, it will override the one defined in the suite).

## Exercise
In this exercise you will be modifying the `exercise1.xml` suite to prepare a full test of the `index.html` page step by step.

#### 1. Screenshot comparison 
Update the test from `exercise1.xml` to obtain a report that will present differences between the `index.html` page *screenshots* in the 1200x800 resolution.

Perform the following steps:
   * add the [open](https://github.com/Cognifide/aet/wiki/Open) command at the beginning of the `collect` phase, please **remember that the page must be opened before any collection happens**,
   * add [screen collector](https://github.com/Cognifide/aet/wiki/ScreenCollector) to capture a page screenshot,
   * before taking a screenshot modify the view [resolution](https://github.com/Cognifide/aet/wiki/ResolutionModifier) to 1200 x 800 px,
   * it is a good practice to let the page adjust to a new resolution before capturing a screenshot, wait 1000 ms (1 second) using [sleep modifier](https://github.com/Cognifide/aet/wiki/SleepModifier),
   * add [layout comparator](https://github.com/Cognifide/aet/wiki/LayoutComparator) that will compare all collected screenshots to the pattern using the `layout` comparator,
   * add [wait-for-page modifier](https://github.com/Cognifide/aet/wiki/WaitForPageLoadedModifier) before capturing a screenshot to be sure the page DOM is loaded.

[Run suite](#running-suite) and check your report.

------

#### 2. JS Errors 
Update the test from `exercise1.xml` so that it checks *js errors*.

Perform the following steps:
   * add [js-errors collector](https://github.com/Cognifide/aet/wiki/JSErrorsCollector),
   * add [js-error comparator](https://github.com/Cognifide/aet/wiki/JSErrorsComparator) that will check all collected js errors and decide if they are expected ones or not.

[Run suite](#running-suite) and check your report.

You will notice that you have an error in your report: 

![JS Error](assets/report-js-error.png)

Assume that this error is expected because it comes from a 3rd party vendor and it does not work on the environment that we are using to test.

Let's fix it. You may filter it using [JS Errors Data Filter](https://github.com/Cognifide/aet/wiki/JSErrorsDataFilter) 
by discarding every error in the `bootstrap.min.js` file using the `source` parameter (a full path including `http://` must be defined - the same value as in the `Source` column).

[Run suite](#running-suite) again and check your report.

------

#### 3. Source comparison 
Update the test from `exercise1.xml` so that it checks the page *source*.

Perform the following steps:
   * add [source collector](https://github.com/Cognifide/aet/wiki/SourceCollector),
   * add [source comparator](https://github.com/Cognifide/aet/wiki/SourceComparator) that will compare the page source to the pattern.
   
[Run suite](#running-suite) and check your report.

------

#### 4. W3C Compliance 
Update the test from `exercise1.xml` so that it checks the *w3c* compliance of the page.

Perform the following steps:
   * you already have [source collector](https://github.com/Cognifide/aet/wiki/SourceCollector) added in [step #3](#3-source-comparison) 
   and there is no need to collect it again since the w3c comparator also makes use of the page source to perform a check,
   * add [w3c-html5 comparator](https://github.com/Cognifide/aet/wiki/W3CHTML5Comparator) that will check the w3c compliance of the page.
   
[Run suite](#running-suite) and check your report.

You will see that the test failed again, because there is the `alt` attribute missing in the `<img src="assets/penguin-elegant.png"/>` tag.

Let's assume again, that we want to fuss around this error no more because it is a known-issue of our CMS and we are aware of it.
It should not break our tests every run. We can filter it out using [W3C Html Issues Filter](https://github.com/Cognifide/aet/wiki/W3CHTML5IssuesFilter).
Try to filter this error by the `line` number that is shown in the report.

[Run suite](#running-suite) again and check your report.

------

#### 5. Status codes 
Update the test from `exercise1.xml` so that it checks *status codes* of the page.

Perform the following steps:
   * add [status-codes collector](https://github.com/Cognifide/aet/wiki/StatusCodesCollector),
   * please notice, that [status-codes collector](https://github.com/Cognifide/aet/wiki/StatusCodesCollector) 
   requires [proxy](https://github.com/Cognifide/aet/wiki/SuiteStructure#proxy) run in the `rest` mode, so add the `useProxy="rest"` property
   to the `homepage` test. If you had forgotten to set the proxy parameter you would see the warning:
   
   ```
   [WARN] [Step: `status-codes` (with parameters: {}) thrown an exception when collecting url: /index.html in: `homepage` test. Cause: Cannot collect status codes without using proxy!]
   ```
   
   * add [status-codes comparator](https://github.com/Cognifide/aet/wiki/StatusCodesComparator) that will look for all the status codes within the range of 400 to 500.
   
[Run suite](#running-suite) and check your report.

The test failed again. There are two `404` status codes (which are within the 400 - 500 range) displayed. 
We know those issues - they occur only in the test environment because of 3rd party libraries. 
Luckily we can ignore them with little help of [Status Codes Data Filter](https://github.com/Cognifide/aet/wiki/StatusCodesDataFilters).
Exclude all the assets that have `.css` and `.js` extensions from the status codes test. 
In order to do that use the `<exclude>` filter with the `pattern` attribute and set the file extension [regular expression](http://www.regular-expressions.info/).

[Run suite](#running-suite) and check your report.

You will see now, that the excluded 404 status codes are on the `All gathered status codes` list.

------

#### 6. Accessibility
Update the test from `exercise1.xml` so that it checks *accessibility*.

Perform the following steps:
   * add [accessibility collector](https://github.com/Cognifide/aet/wiki/AccessibilityCollector) that will gather page accessibility data,
   * add [accessibility comparator](https://github.com/Cognifide/aet/wiki/AccessibilityComparator) that will check page accessibility.
   
[Run suite](#running-suite) and check your report.

As you can see the test failed again. The reason is the same issue that the w3c comparator found - a lack of the `alt` attribute for the image.
Since we can't do anything to fix it, let's filter it out from our test's results using [Accessibility Data Filter](https://github.com/Cognifide/aet/wiki/AccessibilityDataFilter).
Use the `error` attribute of the filter and paste the description of an error (bolded text form the report): `Img element missing an alt attribute. Use the alt attribute to specify a short text alternative.` there.

[Run suite](#running-suite) and check your report.

------

To learn more about available collectors - [visit our Wiki](https://github.com/Cognifide/aet/wiki/Collectors).