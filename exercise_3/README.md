![Cognifide logo](http://cognifide.github.io/images/cognifide-logo.png)

# Automated Exploratory Testing Workshop - Exercise 3

In this exercise you will learn about interactions with tested web page that helps in various situations:

* [Modifying Cookie](https://github.com/Cognifide/aet/wiki/CookieModifier) to deal with disclaimer,
* [Click Modifier](https://github.com/Cognifide/aet/wiki/ClickModifier) that enables basic interaction with a page to show hidden content.

Please see `exercise3-explained.xml` for detailed explanation of suite XML file structure.

## Running suite
In order to run suite on your local Vagrant machine, run following command in this directory:

`mvn aet:run -DtestSuite=exercise3.xml -Ddomain=DOMAIN_WHERE_EXERCISE_PAGE_IS_HOSTED`

This action will execute `exercise3` suite. 
Please remember about changing `DOMAIN_WHERE_EXERCISE_PAGE_IS_HOSTED` to the real domain where the exercise page is hosted.
You can learn more about running suite in [AET wiki](https://github.com/Cognifide/aet/wiki/RunningSuite) and [Workshop description](https://github.com/Skejven/aet-workshop#running-suite).

## Exercise
In this exercise you will be modifying `exercise3.xml` suite to prepare suite that will test pages with interactive components.

#### 1. Hide cookie disclaimer
Update `close-cookie-disclaimer` test from `exercise3.xml` to capture `contact.html` page *screenshots* in 1200x800 resolution.

[Run suite](#running-suite) and check your report. You will notice cookie disclaimer at the top of a captured screenshot. This element we don't want to test.
After manual closing, cookie disclaimer will not show again thanks to the `hideCookieDisclaimer` cookie. 
When we open `contact.html` page with `hideCookieDisclaimer` value set to `true` we will not see cookie disclaimer.

Perform following steps to get rid of this cookie disclamier:
   * add a cookie using [Cookie Modifier](https://github.com/Cognifide/aet/wiki/CookieModifier), set `hideCookieDisclaimer` with value `true`,
   * please remember, that using [Cookie Modifier](https://github.com/Cognifide/aet/wiki/CookieModifier) should be defined before page [opens](https://github.com/Cognifide/aet/wiki/Open).

[Run suite](#running-suite) and check your report.

You will notice that cookie disclaimer is no longer present on a screenshot.

------

#### 2. Show hidden content using *click* modifier
There is hidden section at `index.html` page. Click `Learn more` button to show it.
Update `test-hidden-content` test from `exercise3.xml` to capture `index.html` page *screenshots* in 1200x800 resolution with hidden section included.

Perform following steps:
   * use [Click Modifier](https://github.com/Cognifide/aet/wiki/ClickModifier) to simulate click on `Learn more` button, use `xpath` to localize button,
   * please remember that clicking is possible only on a visible element - always make sure that page is fully rendered before 
   performing *click* action - `timeout` parameter helps to define a time that [Click Modifier](https://github.com/Cognifide/aet/wiki/ClickModifier) can wait for an element to appear.
    Define its duration at 3000 ms.
   
[Run suite](#running-suite) and check your report.

You will notice that captured screenshot contains section that was hidden before.