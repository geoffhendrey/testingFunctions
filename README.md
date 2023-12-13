This readme is a recording of a stated REPL session ...

```shell
> .note - let's show the serverless functions as expressions
"============================================================="
> .open
1: serverlessFuncsAsExpressions.json
2: serverlessFuncsAsFunctions.json
3: testWfExprs.yaml
4: testWfFuncs.yaml
Enter the number of the file to open (or type "abort" to cancel): 1
```
```json
{
  "functions": [
    {
      "name": "is-adult",
      "operation": "applicant.age >= 18",
      "type": "expression"
    },
    {
      "name": "is-minor",
      "operation": "applicant.age < 18",
      "type": "expression"
    }
  ]
}
```
```shell
> .note - let's show the serverless functions as functions
"============================================================="
> .open
1: serverlessFuncsAsExpressions.json
2: serverlessFuncsAsFunctions.json
3: testWfExprs.yaml
4: testWfFuncs.yaml
Enter the number of the file to open (or type "abort" to cancel): 2
```
```json
{
  "functions": [
    {
      "name": "is-adult",
      "operation": "${   function($applicant){$applicant.age >= 18}  }",
      "type": "expression"
    },
    {
      "name": "is-minor",
      "operation": "${   function($applicant){$applicant.age < 18}  }",
      "type": "expression"
    }
  ]
}
```
```shell
> .note - let's show the test when the serverless functions are expressions
"============================================================="
> .open
1: serverlessFuncsAsExpressions.json
2: serverlessFuncsAsFunctions.json
3: testWfExprs.yaml
4: testWfFuncs.yaml
Enter the number of the file to open (or type "abort" to cancel): 3
```
```yaml
{
  "libFuncs": "${$import('https://raw.githubusercontent.com/geoffhendrey/testingFunctions/main/src/serverlessFuncsAsExpressions.json')}",
  "testData": [
    {
      "age": 17
    },
    {
      "age": 18
    },
    {
      "age": 19
    }
  ],
  "isAdult": "${ function($applicant){ $eval(libFuncs.*[name='is-adult'].operation, {'applicant':$applicant})}   }",
  "isMinor": "${ function($applicant){ $eval(libFuncs.*[name='is-minor'].operation, {'applicant':$applicant})}   }",
  "isAdultPassedTests": "${ testData ~> $map(isAdult) = [false, true, true] }",
  "isMinorPassedTests": "${ testData ~> $map(isMinor) = [true, false, false]}"
}
...try '.out' or 'template.output' to see evaluated template
> .out
  {
    "libFuncs": {
      "functions": [
        {
          "name": "is-adult",
          "operation": "applicant.age >= 18",
          "type": "expression"
        },
        {
          "name": "is-minor",
          "operation": "applicant.age < 18",
          "type": "expression"
        }
      ]
    },
    "testData": [
      {
        "age": 17
      },
      {
        "age": 18
      },
      {
        "age": 19
      }
    ],
    "isAdult": "{function:}",
    "isMinor": "{function:}",
    "isAdultPassedTests": true,
    "isMinorPassedTests": true
  }
```
```shell
> .note - let's show the test when the serverless functions are jsonata functions
"============================================================="
> .open
1: serverlessFuncsAsExpressions.json
2: serverlessFuncsAsFunctions.json
3: testWfExprs.yaml
4: testWfFuncs.yaml
Enter the number of the file to open (or type "abort" to cancel): 4
```
```yaml
{
  "libFuncs": "${$import('https://raw.githubusercontent.com/geoffhendrey/testingFunctions/main/src/serverlessFuncsAsFunctions.json')}",
  "testData": [
    {
      "age": 17
    },
    {
      "age": 18
    },
    {
      "age": 19
    }
  ],
  "isAdult": "${libFuncs.*[name='is-adult'].operation}",
  "isMinor": "${libFuncs.*[name='is-minor'].operation}",
  "isAdultPassedTests": "${ testData ~> $map(isAdult) = [false, true, true] }",
  "isMinorPassedTests": "${ testData ~> $map(isMinor) = [true, false, false]}"
}
```
```shell
...try '.out' or 'template.output' to see evaluated template
> .out
```
```json
{
  "libFuncs": {
    "functions": [
      {
        "name": "is-adult",
        "operation": "{function:}",
        "type": "expression"
      },
      {
        "name": "is-minor",
        "operation": "{function:}",
        "type": "expression"
      }
    ]
  },
  "testData": [
    {
      "age": 17
    },
    {
      "age": 18
    },
    {
      "age": 19
    }
  ],
  "isAdult": "{function:}",
  "isMinor": "{function:}",
  "isAdultPassedTests": true,
  "isMinorPassedTests": true
}
```
```shell
> .note you can interact directly with functions for debugging
"============================================================="
> await template.output.isAdult({"age":100})
true
> await template.output.isAdult({"age":15})
false
```