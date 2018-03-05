
|datadog metric name|milestone|required|metric type|units|definition|notes|
|-|-|-|-|-|-|-|
|cicd.duration|ALL|1|real|seconds|duration of the current run|always emit duration from the start of a ci or cd run as we can compare it to the sum of constituent parts to see if there are inter-stage delays.  Always emit a value for the tag cicd_type - in order to determine whether it is a ci run or a cd run.  always emit all four levels of the component taxonomy (division, product, system, component).  the duration is from the time of merge into GM (Gold Master) through to whenever the last milestone happened in the current run.|
|cicd.buildDuration|build|1|real|seconds|build duration||
|cicd.buildSuccess|build|1|real|count|did the build work?||
|cicd.buildFailure|build|1|real|count|did the build fail?||
|cicd.testDuration|test|1|real|seconds|test duration||
|cicd.testSuccess|test|1|real|count|did the test run succeed?||
|cicd.testFailure|test|1|real|count|did the test run fail?||
|cicd.dryRunDuration|dryRunTest|0|real|seconds|duration of the dry run test|This is an optional milestone in a ci or cd run.  It can be implemented by the CDC component (or others).  The intent is that this represents a test using real production traffic but it is impossible for this test to have an impact on production (or customers).  This is the way that CDC curently works.|
|cicd.dryRunSuccess|dryRunTest|0|real|count|did the dry run succeed?||
|cicd.dryRunFailure|dryRunTest|0|real|count|did the dry run fail?||
|cicd.deploySuccess|cd deploy|1|real|count|did the deployment succeed?|whether you are using a firstDeploy (canary) or jump directly to full deployment (finalDeploy), at the start of this phase (or phases), from this point forward you MUST emit a cicd.deploySuccess or cicd.deployFailure.
NOTE: there is no deployDuration because we have the cicd.duration metric which covers everything.  we just wanted an explicit deploySuccess, deployFailure anytime we start an actual deployment.  this means we could have post gold master build/test cycles that fail before we start a deploy.  this is okay but undesirable.  if we think this is happening a lot, we can compare the number of gold master builds to the number of deploys and when they differ, ask ourselves if we care.|
|cicd.deployFailure|cd deploy|1|real|count|did the deployment fail?||
|cicd.firstDeployDuration|firstDeploy|1|real|seconds|duration of the first deployment|this stage represents a canary deployment.  It is optional.  but whether this is a canary that you are measuring or any time that you are deploying - at the start of first deploy (or start of final deploy) going forward you MUST increment cicd.deploySuccess (or cicd.deployFailure)|
|cicd.firstDeploySuccess|firstDeploy|1|real|count|did the first deployment succeed?||
|cicd.firstDeployFailure|firstDeploy|1|real|count|did the first deployment fail?||
|cicd.rollBackDuration|rollback|0|real|seconds|duration of the rollback|At any time after a deployment has occurred to any number of servers, if a there is a failure, then you MUST rollback.  These metrics capture that rollback data.|
|cicd.rollBackSuccess|rollback|0|real|count|did the rollback succeed?||
|cicd.rollBackFailure|rollback|0|real|count|did the rollback fail?||
|cicd.validateFirstDeployDuration|validateFirstDeploy|1|real|seconds|duration of the validation of the first deployment|metrics validating first deployment (aka canary)|
|cicd.validateFirstDeploySuccess|validateFirstDeploy|1|real|count|did the validation of the first deployment succeed?||
|cicd.validateFirstDeployFailure|validateFirstDeploy|1|real|count|did the validation of the first deployment fail?||
|cicd.finalDeployDuration|finalDeploy|1|real|seconds|duration of the first deployment|this is the duration of the rest of deployment. It starts when validateFirstDeploy ends - or it starts when you start deploying (if you're not using a firstDeploy / validateFirstDeploy pair)|
|cicd.finalDeploySuccess|finalDeploy|1|real|count|did the first deployment succeed?||
|cicd.finalDeployFailure|finalDeploy|1|real|count|did the first deployment fail?||
|cicd.validateFinalDeployDuration|validateFinalDeploy|1|real|seconds|duration of the validation of the final deployment|metrics validating the final deployment|
|cicd.validateFinalDeploySuccess|validateFinalDeploy|1|real|count|did the validation of the final deployment succeed?||
|cicd.validateFinalDeployFailure|validateFinalDeploy|1|real|count|did the validation of the final deployment fail?||
