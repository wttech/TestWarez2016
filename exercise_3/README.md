![Cognifide logo](http://cognifide.github.io/images/cognifide-logo.png)

# Automated Exploratory Testing Workshop - Exercise 3

In this exercise you will learn about interactions with a tested web page that helps in various situations:

* [Modifying Cookie](https://github.com/Cognifide/aet/wiki/CookieModifier) to deal with the disclaimer,
* [Click Modifier](https://github.com/Cognifide/aet/wiki/ClickModifier) that enables a basic interaction with a page to show its hidden content.

Please see `exercise3-explained.xml` for a detailed explanation of the structure of the suite XML file.

## Running suite
In order to run the suite on your local Vagrant machine, run the following command in this directory:

`mvn aet:run -DtestSuite=exercise3.xml -Ddomain=http://zg.cognifide.com/aet/testWarez2016`

This action will execute the `exercise3` suite.
You can learn more about running the suite at [AET wiki](https://github.com/Cognifide/aet/wiki/RunningSuite) and [Workshop description](https://github.com/Skejven/aet-workshop#running-suite).

Please notice, that the parameter `domain` can also be defined in the suite XML file (see `exercise1-explained.xml` for a detailed explanation).
When the domain is specified in XML, you may run AET with no `-Ddomain=http://zg.cognifide.com/aet/testWarez2016` parameter (if you do so, it will override the one defined in the suite).

## Exercise
In this exercise you will be modifying the `exercise3.xml` suite to prepare the suite that will test pages containing interactive components.

#### 1. Hide cookie disclaimer
Update the `close-cookie-disclaimer` test from `exercise3.xml` to capture the `contact.html` page *screenshots* in the 1200x800 resolution.

[Run suite](#running-suite) and check your report. You will notice a cookie disclaimer at the top of the captured screenshot. We don't want to test this element.
After closing it manually, the cookie disclaimer will not show again thanks to the `hideCookieDisclaimer` cookie.
When we open the `contact.html` page with the `hideCookieDisclaimer` value set to `true` we will not see the cookie disclaimer.

Perform the following steps to get rid of this cookie disclamier:
   * add a cookie using [Cookie Modifier](https://github.com/Cognifide/aet/wiki/CookieModifier), set `hideCookieDisclaimer` with the value `true`,
   * please remember, that using [Cookie Modifier](https://github.com/Cognifide/aet/wiki/CookieModifier) should be defined before the page [opens](https://github.com/Cognifide/aet/wiki/Open).

[Run suite](#running-suite) and check your report.

You will notice that the cookie disclaimer is no longer present on the screenshot.

------

#### 2. Show hidden content using *click* modifier
There is a hidden section on the `index.html` page. Click the `Learn more` button to show it.
Update the `test-hidden-content` test from `exercise3.xml` to capture `index.html` page *screenshots* in the 1200x800 resolution with the hidden section included.

Perform the following steps:
   * use [Click Modifier](https://github.com/Cognifide/aet/wiki/ClickModifier) to simulate the click action on the `Learn more` button, use `xpath` to localize the button,
   * please remember that clicking is possible only on a visible element - always make sure that the page is fully rendered before 
   performing the *click* action - the `timeout` parameter helps to define the time that [Click Modifier](https://github.com/Cognifide/aet/wiki/ClickModifier) can wait for an element to appear on the page.
   Define its duration as 3000 ms.
   
[Run suite](#running-suite) and check your report.

You will notice that the captured screenshot contains the section that was hidden before.