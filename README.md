This is a sample project that reproduces the issue in IntelliJ editor.

## Steps to reproduce
1. Open the project in IntelliJ
2. Open the file `utils/sum.test.js`
3. Click on the "Run" gutter icon next to the `describe` or `test` block

## Expected behavior
The test should run and pass.

## Actual behavior
The test fails with the following error:
```
No tests were found
```

## Observations
The test runner in the IDE outputs this command:
```shell
/Users/tim.shilov/.volta/bin/node /Users/tim.shilov/IdeaProjects/intellij-vitest-bug-repro/node_modules/vitest/vitest.mjs --run --reporter ../../Applications/IntelliJ IDEA Ultimate.app/Contents/plugins/javascript-impl/helpers/vitest-intellij/node_modules/vitest-intellij-reporter-safe.js --testNamePattern=^ sum  /Users/tim.shilov/IdeaProjects/intellij-vitest-bug-repro/utils/sum.test.js
```
If I just run the same command in terminal I get the following error
```
cat: ../../Applications/IntelliJ: No such file or directory
cat: IDEA: No such file or directory
cat: Ultimate.app/Contents/plugins/javascript-impl/helpers/vitest-intellij/node_modules/vitest-intellij-reporter-safe.js: No such file or directory
```
I believe the issue is that the path to the reporter is not escaped properly. If I escape the spaces in the path, the command works:
```shell
/Users/tim.shilov/.volta/bin/node /Users/tim.shilov/IdeaProjects/intellij-vitest-bug-repro/node_modules/vitest/vitest.mjs --run --reporter ../../Applications/IntelliJ\ IDEA\ Ultimate.app/Contents/plugins/javascript-impl/helpers/vitest-intellij/node_modules/vitest-intellij-reporter-safe.js --testNamePattern=^ sum  /Users/tim.shilov/IdeaProjects/intellij-vitest-bug-repro/utils/sum.test.js
```
And I get the following output:
```
##teamcity[enteredTheMatrix]
##teamcity[testingStarted]
##teamcity[testSuiteStarted nodeId='1' parentNodeId='0' name='sum.test.js' running='true' nodeType='file' locationHint='file:///Users/tim.shilov/IdeaProjects/intellij-vitest-bug-repro/utils/sum.test.js']
##teamcity[testSuiteStarted nodeId='2' parentNodeId='1' name='sum' running='true' nodeType='suite' locationHint='suite:///Users/tim\.shilov/IdeaProjects/intellij-vitest-bug-repro/utils/sum\.test\.js.sum']
##teamcity[testStarted nodeId='3' parentNodeId='2' name='adds 1 + 2 to equal 3' running='true' nodeType='test' locationHint='test:///Users/tim\.shilov/IdeaProjects/intellij-vitest-bug-repro/utils/sum\.test\.js.sum.adds 1 + 2 to equal 3']
##teamcity[testFinished nodeId='3' duration='1']
##teamcity[testSuiteFinished nodeId='2']
##teamcity[testSuiteFinished nodeId='1']
##teamcity[testingFinished]
```
