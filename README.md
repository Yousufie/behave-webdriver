# behave-webdriver
Boilerplate to easily run [selenium](https://github.com/SeleniumHQ/selenium) webdriver tests with the [behave](https://github.com/behave/behave) BDD testing framework
Inspired by the webdriverio [cucumber-boilerplate](https://github.com/webdriverio/cucumber-boilerplate) project.

[![Build Status](https://travis-ci.org/spyoungtech/behave-webdriver.svg?branch=master)](https://travis-ci.org/spyoungtech/behave-webdriver)


# Status

This project is currently in the very early stages of development, but is being worked on regularly. A formal release will be forthcoming.
The current travis build test a base set of features found in tests/features/sampleSnippets.feature
Currently we test against Python 2.7 and Python 3.5+ using a headless chromedriver -- Other versions/browsers may work, but are untested. If you have issues with untested versions or browsers, please raise an issue.



# Goals
- Make writing readable selenium tests as Gherkin features easy.
- Provide an easily extensible interface to the selenium driver (`BehaveDriver`)
- To be (at least mostly) compatible with feature files written for cucumber/webdriverio


# Installation

Via pip (PyPI coming soon)
```
pip install git+https://github.com/spyoungtech/behave-webdriver.git
```

Manual install (setup.py)

```
git clone https://github.com/spyoungtech/behave-webdriver.git
cd behave-webdriver
pip install .
```

# How to use

## Quick start

Basic use of this step library will require you to (1) import the step functions and (2) provide a `behave_driver` to your `context`.

For example, you can import all the step definitions from this library by placing the following in one of your step files (`features/steps`)

```python
# features/steps/webdriver_example.py
from behave_webdriver.steps import *
```

These steps will expect to be able to access a `BehaveDriver` instance in your context. You can provide this, for example, in your `environment.py` file in a `before_all` function.

You can use one of the supplied presets. For testing, we use **Headless Chrome** `BehaveDriver.headless_chrome()`which will work in headless environments, like CI builds.

```python
# features/environment.py
from behave_webdriver import BehaveDriver

def before_all(context):
    # use the headless chromedriver preset
    context.behave_driver = BehaveDriver.headless_chrome()

def after_all(context):
    # cleanup after tests run
    context.behave_driver.quit()
```
You can also supply your own instance of any selenium webdriver by passing the driver to `BehaveDriver` (e.g. `BehaveDriver(my_driver_instance)`

```python
from behave_webdriver import BehaveDriver
from selenium import webdriver

firefox = webdriver.Firefox()

def before_all(context):
    context.behave_driver = BehaveDriver(firefox)
```

## Advanced usage; modifying/extending behave-webdriver

Most of the logic for behave-webdriver is implemented in the `BehaveWebdriver` class. This means you can subclass this to override or extend any behaviors implemented there.
Also worth noting is that you can access attributes of the underlying webdriver directly through `behave_driver` or by accessing the stored driver instance `behave_driver.driver`


For more information about behave, check out the [behave documentation](http://behave.readthedocs.io/en/latest/)


# List of steps implemented

Given and Then steps use the behave regex matcher, When steps use the default parser matcher.
Not all steps have been implemented yet. Steps that are not implemented will be in a separate list.

## Given Steps

- `I open the url "([^"]*)?"`
- `the element "([^"]*)?" is( not)* visible`
- `the element "([^"]*)?" is( not)* enabled`
- `the element "([^"]*)?" is( not)* selected`
- `the checkbox "([^"]*)?" is( not)* checked`
- `there is (an|no) element "([^"]*)?" on the page`
- `the title is( not)* "([^"]*)?"`
- `the element "([^"]*)?" contains( not)* the same text as element "([^"]*)?"`
- `the element "([^"]*)?"( not)* matches the text "([^"]*)?"`
- `the element "([^"]*)?"( not)* contains the text "([^"]*)?"`
- `the element "([^"]*)?"( not)* contains any text`
- `the element "([^"]*)?" is( not)* empty`
- `the page url is( not)* "([^"]*)?"`
- `the( css)* attribute "([^"]*)?" from element "([^"]*)?" is( not)* "([^"]*)?"`
- `the cookie "([^"]*)?" contains( not)* the value "([^"]*)?"`
- `the cookie "([^"]*)?" does( not)* exist`
- `the element "([^"]*)?" is( not)* ([\d]+)px (broad|tall)`
- `the element "([^"]*)?" is( not)* positioned at ([\d]+)px on the (x|y) axis`
- `I have a screen that is ([\d]+) by ([\d]+) pixels`

- `a (alertbox|confirmbox|prompt) is( not)* opened`

### Given steps not yet implemented

- `I have closed all but the first (window|tab)`


## When Steps

- `I pause for {miliseconds:d}ms`
- `I click on the element "{element}"`
- `I doubleclick on the element "{element}"`
- `I click on the link "{link_text}"`
- `I click on the button "{element}"`
- `I set "{value}" to the inputfield "{element}"`
- `I set {value} to the inputfield "{element}"`
- `I add "{value}" to the inputfield "{element}"`
- `I clear the inputfield "{element}"`
- `I drag element "{from_element}" to element "{to_element}"`
- `I submit the form "{element}"`
- `I set a cookie "{cookie_key}" with the content "{value}"`
- `I delete the cookie "{cookie_key}`
- `I press "{key}"`
- `I accept the alert`
- `I dismiss the alert`
- `I enter {text} into the prompt`
- `I scroll to element {element}`


### When steps not yet implemented

- `I close the last opened tab`
- `I close the last opened window`
- `I select the {nth:d} option for element "{element}"`
- `I select the option with the text {text} for element {element}`
- `I select the option with the value {value} for element {element}`
- `I move to element "{element}" with an offset of ({x_offset:d},{y_offset:d})`
- `I move to element "{element}"`


## Then Steps


- `I expect that the title is( not)* "([^"]*)?"`
- `I expect that element "([^"]*)?" is( not)* visible`
- `I expect that element "([^"]*)?" becomes( not)* visible`
- `I expect that element "([^"]*)?" is( not)* within the viewport`
- `I expect that element "([^"]*)?" does( not)* exist`
- `I expect that element "([^"]*)?"( not)* contains the same text as element "([^"]*)?"`
- `I expect that element "([^"]*)?"( not)* matches the text "([^"]*)?"`
- `I expect that element "([^"]*)?"( not)* contains the text "([^"]*)?"`
- `I expect that element "([^"]*)?"( not)* contains any text`
- `I expect that element "([^"]*)?" is( not)* empty`
- `I expect that the url is( not)* "([^"]*)?"`
- `I expect that the path is( not)* "([^"]*)?"`
- `I expect the url to( not)* contain "([^"]*)?"`
- `I expect that the( css)* attribute "([^"]*)?" from element "([^"]*)?" is( not)* "([^"]*)?"`
- `I expect that checkbox "([^"]*)?" is( not)* checked`
- `I expect that element "([^"]*)?" is( not)* selected`
- `I expect that element "([^"]*)?" is( not)* enabled`
- `I expect that cookie "([^"]*)?"( not)* contains "([^"]*)?"`
- `I expect that cookie "([^"]*)?"( not)* exists`
- `I expect that element "([^"]*)?" is( not)* ([\d]+)px (broad|tall)`
- `I expect that element "([^"]*)?" is( not)* positioned at ([\d]+)px on the (x|y) axis`
- `I wait on element "([^"]*)?"(?: for (\d+)ms)*(?: to( not)* (be checked|be enabled|be selected|be visible|contain a text|contain a value|exist))*`
- `I expect that a (alertbox|confirmbox|prompt) is( not)* opened`

### Then steps not yet implemented

- `I expect that element "([^"]*)?" (has|does not have) the class "([^"]*)?"`
- `I expect a new (window|tab) has( not)* been opened`
- `I expect the url "([^"]*)?" is opened in a new (tab|window)`
- `I expect that element "([^"]*)?" is( not)* focused`
- `I expect that a (alertbox|confirmbox|prompt)( not)* contains the text "([^"]*)?"`

# TODO
- [ ] Implement the support code from cucumber-boilerplate for behave/selenium
  - [ ] given 20/21 steps implemented
  - [ ] when  18/25 steps implemented
  - [ ] then 23/28 steps implemented
  - [x] sampleSnippets.feature passing
- [ ] More extensive tests and feature files
- [x] Setup basic travis tests
- [ ] Provide some cool browser options
- [ ] ???
- [ ] Profit

# Acknowledgements

Thanks to the authors of the webdriverio [cucumber-boilerplate](https://github.com/webdriverio/cucumber-boilerplate) project for boilerplate ideas and feature files