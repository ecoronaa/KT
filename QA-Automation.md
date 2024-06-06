# [10 Quality metrics that let you know your automation is successful](https://theqaconnection.com/2021/12/10/10-quality-metrics-that-let-you-know-your-automation-is-successful/)

- Automatable test cases
  num-of-automatable-tests / num-of-total-tests * 100
- Automation Script Effectiveness
  num-of-defects-found-by-automation / num-of-defects-opened * 100
- Automation Pass Rate
  num-of-cases-that-passed / num-of-test-cases-executed * 100
- Automation Execution Time
- Automation Test Coverage
  num-of-automation-tests / num-of-total-tests * 100
- Automation Stability
  *per test* num-of-failure / num-of-executions * 100
- Build Stability
  num-of-build-failures / n-of-builds * 100
- In-sprint automation
  n-of-scripts-created-in-sprint / n-of-scripts-created-post-sprint * 100
- Automation progress
  n-of-automated-tests / n-of-tests-that-are-automatable * 100
- Automation Pyramid
  n-of-tests-at-each-level / n-of-total-tests * 100

# [Testing](https://automationpanda.com/testing/)

There are 3 fundamental categories of software testing
- Functional. *Does the software work correctly?*
- Performance. *Does the software work within desired system metrics?*
- Experimental. *Does the software work with improved feature metrics after a change?*

There are two *modes* of functional testing:
- Scripted: Test cases are written, reviewed, ant then run according to plan.
- Exploratory: Experts interact with product features without a script in attempts to uncover issues.

There are also two methods for running functional tests:
- Automated
  - Good for regressions
  - Fit for scripted tests
- Manual
  - Good to find bugs in new features
  - Fit for exploratory tests and for scripted tests that are difficult to automate.

Two access levels for functional testing:
- White-box: Tests interact directly with product code.
- Black-box: Tests interact with a live instance of the built product.

Three layers for functional testing:
- Unit: white-box (functions, methods, classes)
- Integration: black-box (often service layer)
- End-to-end: black-box (often web ui layer)

# [5 things i love about SpecFlow](https://automationpanda.com/2018/05/10/5-things-i-love-about-specflow/)

## 4. Thorough Outline Templating

All BDD frameworks can parametrize step inputs (shows in double quotes). However, SpecFlow can also parametrize the non-input parts of a step!

```
Feature: Cucumber Basket
  
  Scenario Outline: Use the cucumber basket
    Given the basket has "<initial>" cucumbers
    When "<some>" cucumbers are <handled-with> the basket
    Then the basket has "<total>" cucumbers

    Examples: Counts
      | initial | some | handled-with | total |
      | 1       | 2    | added to     | 3     |
      | 5       | 3    | removed from | 2     |
```

```
[When(@"""(\d+)"" cucumbers are added to the basket")]
public void WhenCucumbersAreAddedToTheBasket(int count)
{
    // ...
}
 
[When(@"""(\d+)"" cucumbers are removed from the basket")]
public void WhenCucumbersAreRemovedFromTheBasket(int count)
{
    // ...
}
```

# [10 gotchas for automation code reviews](https://automationpanda.com/2017/05/08/10-gotchas-for-automation-code-reviews/)

## 10. No proof of success

- proof of success (or not success) must be attached to the review.

## 9. Typos and Bad Formatting

## 8. Hard-coded values

- Should this be a shared constant?
- Should this be a parameterized value for the method/function/step using it?
- Should this be passed into the test as an external input (such as from a config file or the command line)?

## 7. Incorrect Test Coverage

- When reviewing tests, keep the original test procedure handly, and watch out for missing coverage.

## 6. Inadequate Documentation

- Automated test cases should read like tests procedures. **This is one reason why [self-documenting behavior-driven-test-frameworks](https://automationpanda.com/2017/02/04/bdd-101-frameworks/) are so popular**

## 5. Poor code placement

- Maintain a good organized structure of the project.
- Common code should be abstracted from test cases and put into shared libraries.
- Curate your dependencies. E.g., non-web tests should not have a dependency on Selenium WebDriver.

## 4. Bad config changes

As a general rule, submit any config changes in a separate code review from other changes, and provide a thorough explanation to the reviewers for why the change is needed. Any time I see unusual config changes, I always call them out.

## 3. Framework hacks

A framework is meant to help engineers automate tests. However, sometimes the framework may also be a hindrance. Rather than improve the framework design, many engineers will try to hack around the framework. Sometimes, the framework may already provide the desired feature! **Hacks should be avoided because test automation projects need a strong overall design strategy**.

## 2. Brittleness

Test automation must be robust enough to handle bumps in the road.

- Do test case have adequate cleanup routines, even when they crash?
- Are all exceptions handled properly, even unexpected ones?
- Is Selenium WebDriver always disposed?
- Will SSH connections be automatically reconnected if dropped?
- Are XPaths too loose or to strict?
- Is a REST API response code of 201 just as good as 200?

## 1. Duplication

# Side notes

- [The Software Engineer in Test (SET) or Software Development Engineer in Test (SDET)](https://automationpanda.com/2018/10/02/the-software-engineer-in-test/). Good vibes about SET