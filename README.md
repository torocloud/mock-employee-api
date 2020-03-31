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
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
[
    {
        "firstName": "Annadiane",
        "lastName": "Johannes",
        "email": "ajohannes0@so-net.ne.jp",
        "phoneNumber": "ajohannes0@unc.edu",
        "id": 1
    },
    {
        "firstName": "Harland",
        "lastName": "Digwood",
        "email": "hdigwood1@un.org",
        "phoneNumber": "hdigwood1@youku.com",
        "id": 2
    },
    ...
    {
        "firstName": "Jacenta",
        "lastName": "Duigan",
        "email": "jduigan2@ibm.com",
        "phoneNumber": "jduigan2@vinaora.com",
        "id": 19
    },
    {
        "firstName": "Ewell",
        "lastName": "Vamplus",
        "email": "evamplusj@goodreads.com",
        "phoneNumber": "evamplusj@privacy.gov.au",
        "id": 20
    }
]
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
    "employee": {
        "firstName": "John",
        "lastName": "Doe",
        "email": "jdoe@mail.com",
        "phoneNumber": "(373) 321-3203",
        "jobTitle": "Electrical Engineer",
        "jobDescription": "Engineering",
        "jobSalary": 423.3,
        "employmentHireDate": "2018-10-20T23:00:00+0800",
        "employmentStartDate": "2019-12-03T00:00:00+0800",
        "employmentEndDate": "2020-01-09T00:00:00+0800",
        "id": 25
    }
}
```

`GET /employees/<employeeId>`

Returns a single employee record that matches the given `employeeId`

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/mock-employee-api/employees/25 \
  -H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below.
```
{
    "firstName": "John",
    "lastName": "Doe",
    "email": "jdoe@mail.com",
    "phoneNumber": "(373) 321-3203",
    "jobTitle": "Electrical Engineer",
    "jobDescription": "Engineering",
    "jobSalary": 423.3000,
    "employmentHireDate": "2018-10-20T15:00:00+0800",
    "employmentStartDate": "2019-12-02T16:00:00+0800",
    "employmentEndDate": "2020-01-08T16:00:00+0800",
    "id": 25
}
```

`PATCH /employees/<employeeId>`

Updates the employee record that matches the given `employeeId`

**Sample Request**

**curl**
```
curl -X PATCH \
  http://localhost:8080/api/mock-employee-api/employees/25 \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
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
    "message": "Employee with id '25' has been updated."
}
```

`DELETE /employees/<employeeId>`

Deletes an employee record that matches the `employeeId`

**Sample Request**

**curl**
```
curl -X DELETE \
  http://localhost:8080/api/mock-employee-api/employees/25 \
  -H 'Accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code of `200` with the response payload below:
```
{
    "result": "SUCCESS",
    "message": "Employee with id '25' has been deleted."
}
```
