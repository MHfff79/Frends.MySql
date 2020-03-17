# Frends.Community.MySql

FRENDS Task for connecting to MySql database

- [Installing](#installing)
- [Task](#tasks)
  - [ExecuteQuery](#executequery)
  - [ExecuteNonQuery](#ExecuteNonQuery)
  - [ExecuteScalar](#ExecuteScalar)
- [Building](#building)
- [Contributing](#contributing)
- [Change Log](#change-log)

# Installing

You can install the task via FRENDS UI Task view, by searching for packages. You can also download the latest NuGet package from https://www.myget.org/feed/frends/package/nuget/Frends.MySql and import it manually via the Task view.

# Task

## ExecuteQuery

Execute queries against the MySql database and return result of query in JToken.

### InputQuery Properties
| Property    | Type       | Description     | Example |
| ------------| -----------| --------------- | ------- |
| Connection string | string | MySql database connection string | `server=<<your host>>;uid=<<your username>>;pwd=<<your password>>;database=<<your database name>>;` |
| CommandType | enum<> | Indicates is the CommandText interpreted as text (usually query) or as a stored procedure). Possible values are `Text` and `StoredProcedure`. More info [here]( https://dev.mysql.com/doc/dev/connector-net/8.0/html/P_MySql_Data_MySqlClient_MySqlCommand_CommandType.htm). | `Text`|
| CommandText | string | SQL statement to execute at the data source. Usually query or name of a stored procedure. More info [here]( https://dev.mysql.com/doc/dev/connector-net/8.0/html/P_MySql_Data_MySqlClient_MySqlCommand_CommandText.htm). | `SELECT * FROM Table WHERE field = @paramName` |
| Parameters | array[Query Parameter] | Possible parameters. See bellow. |  |

#### Parameter Properties

| Property    | Type       | Description     | Example |
| ------------| -----------| --------------- | ------- |
| Name | string | Parameter name used in Query property | `username` |
| Value | string | Parameter value | `myUser` |
| Data type | enum<> | Parameter [data type](https://dev.mysql.com/doc/dev/connector-net/8.0/html/T_MySql_Data_MySqlClient_MySqlDbType.htm). | `VARCHAR` |

### Options Properties

| Property    | Type       | Description     | Example |
| ------------| -----------| --------------- | ------- |
| Timeout seconds | int | Query timeout in seconds | `60` |
| Throw error on failure | bool | Specify if Exception should be thrown when an error occurs. If set to *false*, task outcome can be checked from #result.Success property. | `false` 
| MySqlTransactionIsolationLevel | enum<> | Possible Transaction isolation level values are: `Default`, `ReadCommitted`, `None`, `Serializable`, `ReadUncommitted`, `RepeatableRead`. None means that queries are not made inside a transaction. Other levels are explained [here]( https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html) | `None`

### Output
| Property    | Type       | Description     | Example |
| ------------| -----------| --------------- | ------- |
| Success | bool | Indicates wheather or no errors query is executed succesfully. Always true, if Throw error on failure is set to true. | `true` |
| Message | string | Error message. Always null, if Throw error on failure is set to true. | `true` |
| Result | JToken | Query result in JToken | `[{ "Name": "Teela", "Age": 42, "Address": "Test road 123" }, { "Name": "Adam", "Age": 42, "Address": null }]` |

To access query result, use

```
#result.Result
```

## ExecuteNonQuery

Executes non querry ( UPDATE, INSERT, and DELETE) the return value is the number of rows affected by the command. For all other types of statements, the return value is -1.

### Parameter Properties

Same as in ExecuteQuery: See [Query Parameter](#query-parameter)

### Output

| Property    | Type       | Description     | Example |
| ------------| -----------| --------------- | ------- |
| Success | bool | Indicates wheather or no errors query is executed succesfully. Always true, if Throw error on failure is set to true. | `true` |
| Message | string | Error message. Always null, if Throw error on failure is set to true. | `true` |
| Result | Int | the return value is the number of rows affected by the command. For all other types of statements, the return value is -1.  | `5` |

To access query result, use 

```
#result.Result
```

## ExecuteScalar

 Executes the query, and returns the first column of the first row in the result set returned by the query. Extra columns or rows are ignored. 

### Parameter Properties

Same as in ExecuteQuery: See [Query Parameter](#query-parameter) 

### Output

| Property    | Type       | Description     | Example |
| ------------| -----------| --------------- | ------- |
| Success | bool | Indicates wheather or no errors query is executed succesfully. Always true, if Throw error on failure is set to true. | `true` |
| Message | string | Error message. Always null, if Throw error on failure is set to true. | `true` |
| Result | dynamic <int, double, etc.> | The first column of the first row in the result set returned by the query. Extra columns or rows are ignored.  | `5` |

To access query result, use

```
#result.Result
```

# Building

Clone a copy of the repo

`git clone https://github.com/CommunityHiQ/Frends.Community.MySQL.git`

Rebuild the project

`dotnet build`

Run Tests

`dotnet test`

Create a NuGet package

`dotnet pack --configuration Release`

# Contributing

When contributing to this repository, please first discuss the change you wish to make via issue, email, or any other method with the owners of this repository before making a change.

1. Fork the repo on GitHub
2. Clone the project to your own machine
3. Commit changes to your own branch
4. Push your work back up to your fork
5. Submit a Pull request so that we can review your changes

NOTE: Be sure to merge the latest from "upstream" before making a pull request!

# Change Log

| Version | Changes |
| ----- | ----- |
| 1.0.7 | Initial version of MySql Query Task |
