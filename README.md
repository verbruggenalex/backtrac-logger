# backtrac-logger

A repository used to trigger and log backtrac tasks in Jenkins. Allows
batch jobs on different projects. It is currently available at:
http://subsites-ci.ne-dev.eu/job/Backtrac%20run%20batch/build?delay=0sec

## Example usage

This example shows a workflow for taking snapshots in batch before a
deployment. Then you do the actual deployment. And the last step will
create another snapshot of the environment and compare it against the
last one.

### 1. Take pre-deploy snapshots

This command will send a snapshot request to Backtrac for each project ID
passed to the backtrac.project_ids property. After all snapshot requests
are sent it will loop over all job ids aggregeated in the results file.

Then a parralel job will be started to contuously check if any of the jobs
has finished processing. And it will report when it has finished.

```
./vendor/bin/phing backtrac-run-batch \
  -D'backtrac.environment'='production' \
  -D'backtrac.auth_token'='xxxxx' \
  -D'backtrac.project_ids'='xxxxx,xxxxx' \
  -D'backtrac.check_results'='false' \
  -D'backtrac.results_file'='results.txt' \
  -D'backtrac.action'='backtrac-single-snapshot'
```

### 2. Perform your deployment.

Make your deployment to the environment you took snapshots of.


### 3. Take post-deploy snapshots and comparison

This command will send a snapshot/diff request to Backtrac for each project
ID passed to the backtrac.project_ids property. After all snapshot/diff
requests are sent it will loop over all job ids aggregeated in the results
file.

Then a parralel job will be started to contuously check if any of the jobs
has finished processing. And it will report when it has finished.

```
./vendor/bin/phing backtrac-run-batch \
  -D'backtrac.environment'='production' \
  -D'backtrac.auth_token'='xxxxx' \
  -D'backtrac.project_ids'='xxxxx,xxxxx' \
  -D'backtrac.check_results'='false' \
  -D'backtrac.results_file'='results.txt' \
  -D'backtrac.action'='backtrac-compare-self'
```
