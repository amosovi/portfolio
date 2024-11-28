# Weather API Testing Collection

This repository contains a comprehensive Postman collection designed for testing the functionality and reliability of a Weather API. The collection includes a variety of tests covering different aspects of the API, ranging from standard functionality checks to edge cases and performance assessments.

## Table of Contents

- [Overview](#overview)
- [Testing Strategy](#testing-strategy)
  - [Test Structure](#test-structure)
  - [Testing Mechanics](#testing-mechanics)
  - [Technologies Used](#technologies-used)
- [Found Issues](#found-issues)
  - [Issue List](#issue-list)
- [Conclusion](#conclusion)

## Overview

The primary goal of this project is to demonstrate advanced API testing skills by thoroughly evaluating the Weather API's endpoints. The collection is meticulously structured to ensure that all possible scenarios are covered, including valid and invalid inputs, boundary conditions, and performance benchmarks.

## Testing Strategy

### Test Structure

The collection is organized into several folders, each targeting specific functionalities of the API:

1. **Current Weather**: Tests for retrieving current weather data using various location parameters.
2. **Historical Data**: Tests for accessing historical weather data with different date formats and ranges.
3. **Non-Functional**: Tests focusing on performance, security, and error handling.
4. **For Data Runner**: Data-driven tests using external JSON files for bulk testing.

### Testing Mechanics

The testing approach includes the following key aspects:

- **Variable Utilization**: Collection and environment variables like `{{baseUrl}}`, `{{apiKey}}`, and `{{location}}` are used to make the tests dynamic and easily configurable.

- **Reusable Test Library**: A custom `testLibrary` object is defined to store common test functions, promoting code reuse and maintainability. The library includes functions for:

  - Checking status codes (`statusCodeIs`)
  - Verifying response keys (`responseContainsKeys`)
  - Validating data types (e.g., `latitudeLongitudeAreNumbers`)
  - Checking error messages (`errorMessageIncludes`)
  - Assessing performance metrics (`responseTimeIsBelow`, `contentSizeIsBelow`)

- **Data-Driven Testing**: The `For Data Runner` folder includes tests that leverage external data files (in JSON format) to perform bulk testing with various input combinations.

- **Comprehensive Coverage**: Tests are designed to cover positive scenarios (valid inputs), negative scenarios (invalid inputs), edge cases (boundary values), and non-functional requirements (performance and security).

### Technologies Used

- **Postman**: For creating and organizing API requests and tests.
- **JavaScript**: Used within Postman's scripting environment for writing test scripts.
- **JSON**: For structuring request payloads and external data files.
- **Postman Environment Variables**: To manage different configurations and keep sensitive data like API keys secure.
- **Chai Assertion Library**: Integrated within Postman for writing expressive assertions.

## Found Issues

During the testing process, several issues and undocumented behaviors were discovered in the Weather API. These findings are crucial for improving the API's reliability and user experience.

### Issue List

1. **Invalid Location Error for Valid Coordinates**

   - **Description**: When requesting weather data for the coordinates `90,180` (which are valid maximum boundary values for latitude and longitude), the API returns an "Invalid location" error.
   - **Endpoint**: `{{baseUrl}}/90,180/today?key={{apiKey}}`
   - **Expected Behavior**: The API should return the weather data for the given coordinates without errors.
   - **Actual Behavior**: Returns an error indicating the location is invalid.
   - **Criticality**: **High** - This issue affects the API's ability to handle valid boundary inputs, which could lead to incorrect handling of user requests at extreme coordinates.

2. **Date Interpreted as Location When Location is Missing**

   - **Description**: If the location parameter is omitted but a date is provided, the API treats the date as the location.
   - **Endpoint**: `{{baseUrl}}//today?key={{apiKey}}`
   - **Expected Behavior**: The API should return an error stating that the location must be specified.
   - **Actual Behavior**: Treats the date (`today`) as the location and attempts to process the request.
   - **Criticality**: **Medium** - Could lead to unexpected results and confusion for the API users.

3. **Inconsistent Minimum Date Handling**

   - **Description**: The documentation states that the available date range starts from `1950-01-01`. However, when this date is used, the API returns an error stating that only dates from `1970-01-01` are allowed. Moreover, specifying `1970-01-01` results in an error indicating that the requested date is from `1969-12-31`. The earliest workable date is actually `1970-01-02`.
   - **Endpoint**: `{{baseUrl}}/London,UK/1950-01-01?key={{apiKey}}`
   - **Expected Behavior**: The API should accept dates starting from `1950-01-01` as per the documentation.
   - **Actual Behavior**: Returns errors for dates before `1970-01-02`.
   - **Criticality**: **High** - This discrepancy between the documentation and actual behavior can lead to misuse of the API and unreliable data retrieval.

4. **SQL Injection String Processed Without Error**

   - **Description**: A request containing an SQL injection attempt is processed without any error or warning.
   - **Endpoint**: `{{baseUrl}}/London,UK'; DROP TABLE weather;--?key={{apiKey}}`
   - **Expected Behavior**: The API should sanitize inputs and return an error for malicious input patterns.
   - **Actual Behavior**: Processes the request as if it were a normal location string.
   - **Criticality**: **Critical** - While the API may not be vulnerable to SQL injection, accepting such input without validation poses a security risk and may encourage malicious attempts.

## Conclusion

This project showcases a thorough testing regimen for a Weather API, highlighting both the testing methodologies employed and the issues uncovered. The collection is designed to be comprehensive, covering various input types and scenarios to ensure the API's robustness.

By identifying critical issues and undocumented behaviors, the testing process not only validates the API's functionality but also contributes to its improvement. The use of advanced testing techniques and tools demonstrates a deep understanding of API testing best practices.

---

**Note to Potential Employers**: This repository reflects a commitment to quality assurance and attention to detail in API testing. The approach taken ensures that all aspects of the API are scrutinized, providing valuable feedback for developers and stakeholders.

For any questions or further discussion about this project or testing strategies, please feel free to contact me.

# Contact

[Ilia Amosov](mailto:amosov.ilya@gmail.com)

# Acknowledgments

- Special thanks to the developers of the Weather API for providing a platform to demonstrate advanced testing techniques.
- Thanks to the Postman community for resources and best practices in API testing.
