## Github Actions
Gihub workflow - Set of **Jobs** running *on* **events**

Given the definition - Github workflow .yml file are composed of
- on: events // trigger workflow when the event occurs
- jobs // steps to execute to run the workflow/pipeline

## Events
Ref: https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows

### on.pull_request
```
on: 
 
```

### on.push
On push to `test-workflow` branch run the below jobs
```
on: 
  push: 
    branches: 
      - test-workflow
jobs: 
  ...
```

### on.workflow_call


### on.schedule


## Jobs
Set of jobs to run the commands. Every job would need a
- name
- runs-on (where to run)
- if (conditions)
- steps (steps to run)


### job.steps


### job.if

### job.strategy
### job.concurrency

### job.uses & job.secrets

### Services

## Variables
```
env: 
  APP_VERSION: 1
```
Available at top-level, Job-level and step level.

### Default Variables
Gihub provides some default variables that are accessible across workflows. Some of them are based on *Context* changes
based on which context its running in
Ref: https://docs.github.com/en/actions/learn-github-actions/variables#default-environment-variables
```
- CI
- GITHUB_*
- RUNNER_*
```
Accessed through `$GITHUB_EVENT_NAME

## Github Context variables
Context variable are an extensive set that specific and grouped under different context
- github  // also available in GITHUB_*
- env 
- vars 
- job 
- jobs 
- steps 
- runner 
- secrets 
- strategy 
- matrix 
- needs 
- inputs 

Accessed with `${{ github.event_name }}`

