Welcome to the backend of CVP. At the moment there isn't a whole lot going on, but we can most certainly use your help. The backend consists of a http web server and a SQL database. The http server is designed to be exposed to the internet with the intention that both the iOS app and the website fetch and post data to it.

The API is made up of http methods which read and write data. This model from a design point of view forces a strict separation between how the data's stored and how it's used. Suppose in the future we needed to change the backend database. By separating the API we are able to seemlessly change where the data's store without having to modify either the website or the iOS app.


## Model

The following data structures are going to be vended from the API. The data is seralised using JSON for ease of use and clarity.
- User
- Task

A user has the following fields:

```json
{
	id: Number,
	username: "String",
	firstName: "String",
	lastName: "String",
	phoneNumber: "String",
	address: "String",
	profileDescription: "String",
	passwordHash: "String"
}
```

A task has the following fields:

```json
{
	title: "String",
	id: Number,
	category: "String",
	description: "String",
	datePosted: "String", # what standard is being used for dates?
	completionStatus: Number,
	postedById: Number,
	contactPhone: "String",
	address: "String"
}
```

`completionStatus` is an enumeration with the following values:
| Completion Status | Value |
| ----------------- | ----- |
| Open              | 0     |
| Volunteered       | 1     |
| Completed         | 2     |


## HTTP Methods

To begin with, we're going to follow a standard convention for access the fields from the data structures. Each data structure will live under /api/, for example, tasks will live under /api/tasks. Then, to access the fields of a particular instance, we'll use it an `id` and field name like so: `/api/tasks/<id>/<field-name>`. Substituting in `10` for `<id>` and `address` for `<field-name>` results in requesting the address of the task with identifier 10. The address for that information is `/api/tasks/10/address`. All of the fields of an object are located under the object's id, that is `/api/tasks/10` will return something similar to the above description for a `task`.

Reading and writing is distinguished by `GET` and `POST` requests. A `GET` request performs the above operation, while `POST` requests the field to be modified.

### Examples

This get request returns the entire user object with id 1.
```
HTTP1.1 GET /api/users/1
```
The result might look like this:
```
{
	id: 1,
	username: "mrwerdo",
	firstName: "Andrew",
	lastName: "Thompson",
	phoneNumber: "NNNN-NNN-NNN",
	address: "Street, City, Post Code, State, Country",
	profileDescription: "no description provided",
	passwordHash: "MEYCQQDrKoyL4CITlKPko4hn427R63LpxyFQbuLKizfu2G/CkqQnRmY1/H+36m+3"
}
```

This get request updates the entire user object with id 1 to the values provided.

This post request updates the entire user object with id 1.

```
HTTP1.1 POST /api/users/1


{
	id: 1,
	username: "mrwerdo",
	firstName: "Andrew",
	lastName: "Thompson",
	phoneNumber: "NNNN-NNN-NNN",
	address: "Street, City, Post Code, State, Country",
	profileDescription: "I'm a developer.",
	passwordHash: "MEYCQQDrKoyL4CITlKPko4hn427R63LpxyFQbuLKizfu2G/CkqQnRmY1/H+36m+3"
}
```
