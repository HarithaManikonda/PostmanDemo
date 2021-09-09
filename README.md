# PostmanDemo
This repo has postman collecton to demonistrate the below concepts.

## Different variable scopes
- Global
- Collection
- Environment
- Data
- Local
- Global variables allow you to access data between collections, requests, test scripts, and environments. Global variables are available throughout a workspace.
	- Since global variables can create confusion, you should only use them sparingly—for example to quickly test something or when your project is at a very early prototyping stage.
- Collection variables are available throughout the requests in a collection and are independent of environments, so do not change based on the selected environment.
	- Collection variables are suitable if you are only using a single environment, for example for auth / URL details.
- Environment variables allow you to tailor your processing to different environments, for example local development vs testing or production. Only one environment can be active at a time.
	- If you only have one environment, using collection variables can be more efficient, however environments allow you to specify role-based access levels.
- Local variables are temporary, and only accessible in your request scripts. Local variable values are scoped to a single request or collection run, and are no longer available when the run is complete.
	- Local variables are suitable if you need a value to override all other variable scopes but do not want the value to persist once execution has ended.
- Data variables come from external CSV and JSON files to define data sets you can use when running collections via Newman or the Collection Runner.
## Writing Assertions in postman
assert status code

```

pm.test("Status code is 200", function () {
  pm.response.to.have.status(200);
});
```

assert response body

```
pm.test("Validate response body", () => {
  const responseJson = pm.response.json();
  pm.expect(responseJson[0].author).to.eql("John foer");
});
```
## Using postman collection runner
A postman collection runner is used to perform Data-driven testing. The group of API requests are run in a collection for the multiple iterations with different sets of data.

![Screen Shot 2021-09-07 at 10 32 18 PM](https://user-images.githubusercontent.com/87215340/132451944-91199b60-8156-4899-9e6c-e2262b6cc2bd.png)

## Using result of one API as input to another API
Use a collection variable! If a ‘token’ is returned in the response of Request #1, and you need to use that in Request #2, then something like this will do the trick:
In the ‘test’ script for Request #1, get the value of the response body object with something like…
```
const responseJson = pm.response.json();
const token = responseJson.token

pm.collectionVariables.set('token', token); 
```
Now use this variable as {{token}}  in Request #2

## Using newman to run from commandline or from jenkins
1. Newman is a command-line Collection Runner for Postman. It enables you to run and test a Postman Collection directly from the command line.
2. It is also used to run postman collection in jenkins
3. Newman is built on Node.js
 ```
$ npm install -g newman
```
## Run the collection 10 times with provided data file
 
```
$ newman run mycollection.json -n 10  -d data.json
```
