# [Behavior Driven Development (BDD) - an in-depth look](https://www.dotnetcurry.com/patterns-practices/1375/behavior-driven-development-bdd)

## A quick description and example

```Gherkin
Given a first number 4
  And a second number 3
When the two numbers are added
Then the result is 7
```

```C#
using MathLib;
using NUnit.Framework;
using TechTalk.SpecFlow;
 
namespace MathLibTests
{
    [Binding]
    public sealed class Steps
    {
        [Given(@"a first number (.*)")]
        public void GivenAFirstNumber(int firstNumber)
        {
            ScenarioContext.Current.Add("FirstNumber", firstNumber);
        }
 
        [Given(@"a second number (.*)")]
        public void GivenASecondNumber(int secondNumber)
        {
            ScenarioContext.Current.Add("SecondNumber", secondNumber);
        }
 
        [When(@"the two numbers are added")]
        public void WhenTheTwoNumbersAreAdded()
        {
            var firstNumber = (int)ScenarioContext.Current["FirstNumber"];
            var secondNumber = (int)ScenarioContext.Current["SecondNumber"];
 
            var mathLibOps = new MathLibOps();
            var addResult = mathLibOps.Add(firstNumber, secondNumber);
 
            ScenarioContext.Current.Add("AddResult", addResult);
        }
 
        [Then(@"the result should be (.*)")]
        public void ThenTheResultShouldBe(int expectedResult)
        {
            var addResult = (int)ScenarioContext.Current["AddResult"];
            Assert.AreEqual(expectedResult, addResult);
        }
 
    }
}
```

- `ScenarioContext` is just a dictionary and is used to store data needed for executing the test. This context is cleared at the end of the test.

> As we keep adding tests, the actual code we write becomes smaller because for each system behavior we are testing, we will get to the point where we simply reuse the existing steps we have already coded.

## A complex problem description

We have a website where people can visit and then search and apply for jobs. Restrictions will apply based on their membership type.

- Membership types: Platinum, Gold, Silver, Free
  - Platinum can **search 50 times/day** and **apply 50 jobs/day**
  - Gold: 15 searches, 15 applications
  - Silver: 10 searches, 10 applications
  - Free: 5 searches, 1 application

### What we need

1. define the Users.
2. define the membership types.
3. define the restrictions for every membership type.
4. a wat to retrieve how many searches and applications a user has done every day

### How do we load tabular data in the steps code?

```C#
private List<MembershipTypeModel> GetMembershipTypeModelsFromTable(Table table)
{
    var results = new List<MembershipTypeModel>();
 
    foreach ( var row in table.Rows)
    {
        var model = new MembershipTypeModel();
        model.Restriction = new RestrictionModel();
 
        model.MembershipTypeName = row.ContainsKey("MembershipTypeName") ? row["MembershipTypeName"] : string.Empty;
 
        if (row.ContainsKey("MaxSearchesPerDay"))
        {
            int maxSearchesPerDay = 0;
 
            if (int.TryParse(row["MaxSearchesPerDay"], out maxSearchesPerDay))
            {
                model.Restriction.MaxSearchesPerDay = maxSearchesPerDay;
            }
        }
 
        if (row.ContainsKey("MaxApplicationsPerDay"))
        {
            int maxApplicationsPerDay = 0;
 
            if (int.TryParse(row["MaxApplicationsPerDay"], out maxApplicationsPerDay))
            {
                model.Restriction.MaxApplicationsPerDay = maxApplicationsPerDay;
            }
        }
 
        results.Add(model);
    }
 
    return results;
}
```

**It is a goos idea to always check that a header exists before trying to load anything**. This is very useful because depending on what you're building, you don't always need all the properties and objects at same time.

The actual step for loading the membership types now becomes very trivial:

```C#
[Given(@"the membership types")]
public void GivenTheMembershipTypes(Table table)
{
    var membershipTypes = this.GetMembershipTypeModelsFromTable(table);
    ScenarioContext.Current.Add("MembershipTypes", membershipTypes);
}
```

# Side notes

- [Convert a DataReader to DataTable in ASP.NET](https://www.dotnetcurry.com/aspnet/143/convert-data-reader-to-data-table). SQL connection?
- [Configure properties of Test Plan in Microsoft Test Manager (MTM)](https://www.dotnetcurry.com/visualstudio/674/configure-test-plan-microsoft-test-manager-mtm)
- [Live Unit Testing in Visual Studio 2017](https://www.dotnetcurry.com/visualstudio/1363/live-unit-testing-visual-studio-2017)
- [Testing with Team Web Access using Visual Studio 2013](https://www.dotnetcurry.com/visualstudio/951/testing-team-web-access-visual-studio-2013)
- [Test Hub enhancements in Visual Studio Team Services](https://www.dotnetcurry.com/visualstudio/1313/test-hub-new-features-vsts)

## Important

- [Unit Testing ASP.NET Core Applications](https://www.dotnetcurry.com/aspnet-core/1414/unit-testing-aspnet-core)
- [Integration Testing for ASP.NET Core Applications](https://www.dotnetcurry.com/aspnet-core/1420/integration-testing-aspnet-core)
- [End-to-end (E2E) Testing of ASP.NET Core Applications using selenium and nightwatch.js](https://www.dotnetcurry.com/aspnet-core/1433/end-to-end-testing-aspnet-core)