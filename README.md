# Mock Employee API
A simple employee api with CRUD operations

### Prerequisites

  - Apache Maven 3+
  - [Martini Desktop](https://www.torocloud.com/martini/download)

### Building the Martini Package

```
$ mvn clean package
```
This will create a ZIP file named `mock-employee-api-bin.zip` containing all the files (services, configurations, etc.) needed under the `target` folder. This ZIP file is what we call a [Martini Package](https://docs.torocloud.com/martini/latest/developing/package/) which then you can import in Martini Desktop to get started. You can learn more how to import a Martini Package by visiting our [documentation](https://docs.torocloud.com/martini/latest/developing/package/importing/)

### Getting the Martini Package from TORO Marketplace

You can also get this package via TORO Marketplace. See our documentation on [How to import a Martini Package from Marketplace](https://docs.torocloud.com/martini/latest/developing/package/importing/#from-the-marketplace).

### Usage
This package exposes operations for a simple employee REST API that allows you to do CRUD operations. You can find the [Gloop REST API](https://docs.torocloud.com/martini/latest/developing/gloop/api/rest/) file at `io/toro/mock/employee/api/Employee` under the `code` folder after importing the package to your Martini Desktop application.

### Setup
This Martini Package automatically sets up the required resources for you. During the package startup, Martini will execute the configured _Startup Service_ that initializes the database to be used for storing the record that will be creating for using this mock api.

This package uses HSQL as the default datastore but you can change this to MySQL by updating the [package properties](https://docs.torocloud.com/martini/latest/developing/package/properties/) located at [conf](https://docs.torocloud.com/martini/latest/developing/package/directory/) folder. Below is what the contents of the properties file look like

```
# Flag used for testing
debug.enabled=true

# The database connection name to be used
database.name=mock_employee_api

# The database type to be used
database.type=hsql

# HSQL database configuration to be used for testing environment
hsql.driver=org.hsqldb.jdbc.JDBCDriver
hsql.connection.url=jdbc:hsqldb:file:${toroesb.home}data/hsql/mock_employee_api.db;hsqldb.tx=MVCC;sql.syntax_mys=true
hsql.username=sa
hsql.password=

# MySQL database configuration to be used in production environment
mysql.driver=com.mysql.cj.jdbc.Driver
mysql.connection.url=jdbc:mysql://<host>/<database-to-use>
mysql.username=root
mysql.password=
```
* `debug.enabled` when set to `true` initializes the database with dummy data when the package starts. The initialized data are also removed when the package stops or is unloaded. Set the value of this property to `false` if you want to keep your data when doing a restart. You can also set this property to `true` when the package starts and then set to `false` afterwards so that you'll have the data initialized on startup, and keep the data when the package or the Martini instance do a restart
* `database.type` sets the database provider the Martini package will use. If you will use the default hsql config, you don't need to change anything in the hsql configuration. **Note**: If you will use a different hsql database, make sure that you add `sql.syntax_mys=true` in the connection properties. This ensures that the SQL query from the SQL Services in this package will be compatible with hsql.

### Sending Authenticated Requests

You can use your ECC account to send authenticated request to the API. Your ECC credentials must be sent in the `Authorization` header in the HTTP request

#### To authenticate a request with basic authentication

1. Combine your email and password with a colon (`:`). e.g. `jdoe@mailinator.com:pa$$w0rd`
2. Encode the resulting string in Base64
3. Include an Authorization header in the HTTP request containing the base64-encoded string. Example: ```
Authorization: Basic amRvZUBtYWlsaW5hdG9yLmNvbTpwYSQkdzByZA==```

### Operations

The base url is `<host>/api/mock-employee-api` where `host` is the location where the Martini is deployed. By default, it's `localhost:8080`.

`GET /employee`

Returns a list of employees

**Properties**
* **limit** - Query parameter that sets the limit of the number of records returned by the response
 
**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/mock-employee-api/employees?limit=20 \
  -H 'accept: application/json' \
  -H 'Authorization: Basic <base64-encoded-credentials-string>' \
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "SUCCESS",
    "message": "Successfully retrieved employees.",
    "employees": [
        {
            "firstName": "Terrance",
            "lastName": "Follitt",
            "email": "tfollittg@mlb.com",
            "phoneNumber": "tfollittg@opensource.org",
            "jobTitle": "Electrical Engineer",
            "jobDescription": "Support",
            "jobSalary": 422.45,
            "employmentHireDate": 1540080000000,
            "employmentStartDate": 1575331200000,
            "employmentEndDate": 1578528000000,
            "id": "26960f80-e611-4d43-931d-6f76c6cdf5ff"
        },
        {
            "firstName": "Harland",
            "lastName": "Digwood",
            "email": "hdigwood1@un.org",
            "phoneNumber": "hdigwood1@youku.com",
            "jobTitle": "Compensation Analyst",
            "jobDescription": "Business Development",
            "jobSalary": 525.77,
            "employmentHireDate": 1540080000000,
            "employmentStartDate": 1575331200000,
            "employmentEndDate": 1584576000000,
            "id": "296a9110-0e7d-4f3b-8166-19be883b353d"
        }
    ]
}
```

`POST /employees`

Create a new employee

**Sample Request**

**curl**
```
curl -X POST \
  http://localhost:8080/api/mock-employee-api/employees \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <base64-encoded-credentials-string>' \
  -d '{
    "firstName": "John",
    "lastName": "Doe",
    "email": "jdoe@mail.com",
    "phoneNumber": "(373) 321-3203",
    "jobTitle": "Electrical Engineer",
    "jobDescription": "Engineering",
    "jobSalary": 423.3000,
    "employmentHireDate": "2018-10-20T15:00:00+0800",
    "employmentStartDate": "2019-12-02T16:00:00+0800",
    "employmentEndDate": "2020-01-08T16:00:00+0800"
}'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below.
```
{
    "result": "SUCCESS",
    "message": "Successfully added employee.",
    "employees": {
        "firstName": "Terrance",
        "lastName": "Follitt",
        "email": "tfollittg@mlb.com",
        "phoneNumber": "tfollittg@opensource.org",
        "jobTitle": "Electrical Engineer",
        "jobDescription": "Support",
        "jobSalary": 422.45,
        "employmentHireDate": 1540080000000,
        "employmentStartDate": 1575331200000,
        "employmentEndDate": 1578528000000,
        "id": "26960f80-e611-4d43-931d-6f76c6cdf5ff"
    }
}
```

`GET /employees/<employeeId>`

Returns a single employee record that matches the given `employeeId`

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/mock-employee-api/employees/26960f80-e611-4d43-931d-6f76c6cdf5ff \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <base64-encoded-credentials-string>' \
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below.
```
{
    "result": "SUCCESS",
    "message": "Successfully fetched employee with id 26960f80-e611-4d43-931d-6f76c6cdf5ff.",
    "employees": {
        "firstName": "Terrance",
        "lastName": "Follitt",
        "email": "tfollittg@mlb.com",
        "phoneNumber": "tfollittg@opensource.org",
        "jobTitle": "Electrical Engineer",
        "jobDescription": "Support",
        "jobSalary": 422.45,
        "employmentHireDate": 1540080000000,
        "employmentStartDate": 1575331200000,
        "employmentEndDate": 1578528000000,
        "id": "26960f80-e611-4d43-931d-6f76c6cdf5ff"
    }
}
```

`PATCH /employees/<employeeId>`

Updates the employee record that matches the given `employeeId`

**Sample Request**

**curl**
```
curl -X PATCH \
  http://localhost:8080/api/mock-employee-api/employees/26960f80-e611-4d43-931d-6f76c6cdf5ff \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic <base64-encoded-credentials-string>' \
  -d '{
    "email": "j.doe@mail.com",
    "phoneNumber": "(897) 897-1917",
    "jobTitle": "Software Engineer"
}'
```

**Sample Response** 

If the request is successful, it will return an HTTP status code of `200` with the response payload below.
```
{
    "result": "SUCCESS",
    "message": "Employee with id 26960f80-e611-4d43-931d-6f76c6cdf5ff has been updated.",
    "employees": {
        "firstName": "Terrance",
        "lastName": "Follitt",
        "email": "j.doe@mail.com",
        "phoneNumber": "(897) 897-1917",
        "jobTitle": "Software Engineer",
        "jobDescription": "Support",
        "jobSalary": 422.45,
        "employmentHireDate": 1540080000000,
        "employmentStartDate": 1575331200000,
        "employmentEndDate": 1578528000000,
        "id": "26960f80-e611-4d43-931d-6f76c6cdf5ff"
    }
}
```

`DELETE /employees/<employeeId>`

Deletes an employee record that matches the `employeeId`

**Sample Request**

**curl**
```
curl -X DELETE \
  http://localhost:8080/api/mock-employee-api/employees/26960f80-e611-4d43-931d-6f76c6cdf5ff \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <base64-encoded-credentials-string>' \
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below:
```
{
    "result": "SUCCESS",
    "message": "Employee with id 26960f80-e611-4d43-931d-6f76c6cdf5ff has been deleted."
}
```
