# [A detailed guide to data-driven testing](https://testsigma.com/data-driven-testing)

## What is DDT?

DDT separated test logic (script) form test data (input values). Therefore, DDT is a methodology where a sequence of test steps are automated to run repeatedly for different permutations of data.

Four step process:
1. Fetch input data (.xls, .csv, .xml files)
2. Entering input data into de Application Under Test (AUT).
3. Comparing the actual results with the expected output.
4. Running the same test again with the next row of data.

## Best Practices of DDT

- Testing with Positive and Negative Data.
- Some hygiene tips for DDT
  - Try to form test that create their own scenario test data.
  - Ensure tests don't make any permanent changes to the database.
  - Create automated test scripts that can be easily be modified for various data scenarios.
  - Place test data in one storage format for all test cases

## Test Data Generation

### [Data - the essential element in testing](https://www.synthesized.io/reports/practical-guide-to-data-driven-testing)

- Production data
  - High **risk of privacy leakage, data quality**, testing coverage, **risk of affecting live processes**
  - Medium efficiency and scalability
- Obfuscated subset of production
  - Medium risk of privacy leakage, data quality, testing coverage.
  - Low risk of affecting live processes, efficiency and scalability
- Mock data
  - Low risk of privacy leakage, data quality, testing coverage. risk of affecting live processes.
  - **High efficiency and scalability**.

# Side notes:

- Pugh, Ken. Lean-Agile Acceptance Test-Driven Development: Better Software Through Collaboration. Addison-Wesley, 2011.