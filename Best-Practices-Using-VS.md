# [Agile Development Best Practices Using Visual Studio 2015 and TFS 2015](https://www.dotnetcurry.com/visualstudio/1338/agile-development-best-practices-using-visual-studio)

## Refactoring Code

Refactoring increases the readability of code and decreases complexity. The techniques for refactoring are encapsulating field, extracting a method, renaming, extracting interface, removing parameters.

- **Extracting a method** creates a new method by extracting selected code. A new method is created and the selected code is replaces with the call to the new method.
  > Select the code > right click on it > select Quick Actions and Refactoring > Select method extraction.
- **Rename refactoring**
- **Encapsulating field** refactoring option let us create a property from a field.
  > Right click the line where the public field is declared > Select the menu for refactoring > Select encapsulate the field.
- **Remove parameter** refactoring technique is used to remove parameter from methods/delegates. This step removes the parameter from the signature of the method, but not from the body of the method.
  > Select the method from which to remove the parameter > Select the option for refactoring > change signature

## Code review

There is a direct support of Code Review in TFS or VSTS while writing code with Visual Studio 2015. There is a special wok item of type *Code Review Request* for a developer to initiate code review. The reviewer can then send a Code Review Response in return with the comments and suggestions.

> Visit the main article for detailed steps.

# [Code Quality Tools in Visual Studio 2015](https://www.dotnetcurry.com/visualstudio/1324/code-quality-tools-visual-studio-2015)

**Create unit test with VS contextual menu.**

## Convert Unit Test to Data Driven Unit Test

The data can be provided via xml file, csv file or even via database. E.g.,

`Numbers.xml`
```xml
<?xml version="1.0" encoding="utf-8" ?>
<data>
  <numbers>
    <num1>10</num1>
    <num2>20</num2>
    <result>30</result>
   </numbers>
  <numbers>
    <num1>20</num1>
    <num2>40</num2>
    <result>60</result>
   </numbers>
  <numbers>
    <num1>1</num1>
    <num2>2</num2>
    <result>3</result>
   </numbers>
</data>
```

```C#
[TestClass()]
public class CalcCIsTests
{
    [DataSource("Microsoft.VisualStudio.TestTools.DataSource.XML","|DataDirectory|\\Numbers.xml","numbers",DataAccessMethod.Sequential),TestMethod()]
    public void SumTest()
    {
        CalcCIs = target = new CalcCIs();
        int actual, expected = Int32.Parse(TestContext.DataRow["result"].ToString());
        actual = target.Sum(Int32.Parse(TestContext.DataRow["num1"].ToString()), Int32.Parse(TestContext.DataRow["num2"].ToString()));
        Assert.AreEqual(expected, actual)
    }

    private TestContext testContextInstance;

    public TestContext TestContext
    {
        get
        {
            return testContextInstance;
        }
        set
        {
            testContextInstance = value;
        }
    }
}
```

> Review original article for further details.

# [NUnit Testing with Visual Studio 2015](https://www.dotnetcurry.com/visualstudio/1352/nunit-testing-visual-studio-2015)

## Data Driven NUnit test

```C#
[Test, TestCase(10, 20, 30)]
 public void TestMethodSum(int num1,int num2, int res)
 {
      int actual, expected = res;
      ClassLibNUnit.ClassForNUnit target = new ClassLibNUnit.ClassForNUnit();
       actual = target.Sum(num1, num2);
       NUnit.Core.NUnitFramework.Assert.AreEqual(expected, actual);
}
```

## Sending Arrays as Parameters to the Test

```C#
static int[][] Numbers = { new int[] { 5, 4, 2, 11 }, new int[] { 6, 4, 10 } };

[Test, TestCaseSource("Numbers")]
public void TestSumConstant(int[] num)
{
    int actual, expected = num[num.Length-1];
    ClassLibNUnit.ClassForNUnit  target = new  ClassLibNUnit.ClassForNUnit();
    List<int> param = new List<int>();
    for (int i = 0; i < num.Length-1; i++)
    {
        param.Add(num[i]);
    }
    actual = target.Sum(param.ToArray());
    NUnit.Core.NUnitFramework.Assert.AreEqual(expected, actual);
}
```

## Providing data to the Test using CSV

```
10,10,10,30
1,2,3,4,10
```

```C#
public class GetTestDt
{
    string CSVFileName = @"<path for="" file=""> \data.csv";
    // maybe it is correct
    // string CSVFileName = @"<path for="" file=""></int></int></path> \data.csv";
    public int input { get; set; }
    public int output { get; set; }
    public IEnumerable GetTestData
    {
        get
        {
            using (var inputStream = new FileStream(CSVFileName, FileMode.Open, FileAccess.Read))
            {
                using (var streamReader = new StreamReader(inputStream))
                {
                    string inputLine;
                    while ((inputLine = streamReader.ReadLine()) != null)
                    {
                        var data = inputLine.Split(',');
                        List<int> param = new List<int>();
                        for (int i = 0; i < data.Length ; i++)
                        {
                            param.Add(Convert.ToInt32(data[i]));
                        }
 
                        yield return new[] { param.ToArray() };
                    }
                }
            }
        }
    }
}
// it was here, but i think it goes up, at CSVFileName
// </int></int></path>
```

The test method looks as follows:

```C#
[Test, TestCaseSource(typeof(GetTestDt), "GetTestData")]
public void TestSumCsvFile(int[] num)
{
    int actual, expected = num[num.Length-1];
    ClassLibNUnit.ClassForNUnit target = new ClassLibNUnit.ClassForNUnit();
    List<int> param = new List<int>();
    for (int i = 0; i < num.Length - 1; i++)
    {
        param.Add(num[i]);
    }
    actual = target.Sum(param.ToArray());
    NUnit.Core.NUnitFramework.Assert.AreEqual(expected, actual);
}
```
