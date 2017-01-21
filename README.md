# cf-example-deploy-elasticbeanstalk
[![Codefresh build status]( https://g.codefresh.io/api/badges/build?repoOwner=codefreshdemo&repoName=cf-example-deploy-elasticbeanstalk&branch=master&pipelineName=cf-example-deploy-elasticbeanstalk&accountName=nikolai&type=cf-1)]( https://g.codefresh.io/repositories/codefreshdemo/cf-example-deploy-elasticbeanstalk/builds?filter=trigger:build;branch:master;service:58836950b2f11c0100175173~cf-example-deploy-elasticbeanstalk)

Deployment to Elastic Beanstalk

### Prerequiests
- Configured Application and task definition with an created environment.
  See http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/GettingStarted.html

### Deployment with Codefresh
- Add encrypted environment variables for aws credentials.
     * AWS_ACCESS_KEY_ID
     * AWS_SECRET_ACCESS_KEY
- Provide the following environment variables:
    * AWS_REGION
    * AWS_ENV_NAME
    * AWS_VERSION
    * AWS_BRANCH

![codefresh](./images/codefresh_eb_env_vars.png)

The ``${{AWS_VERSION}}`` of application you can find in the Elastic Beanstalk service
![codefresh](./images/codefresh_eb_version_label.png)

Add the following step to codefresh.yml

```yml
deploy-elastic-beanstalk:
    fail-fast: false
    image: garland/aws-cli-docker:latest
    commands:
     - sh -c  "aws configure set region '${{AWS_REGION}}' && aws elasticbeanstalk update-environment --environment-name '${{AWS_ENV_NAME}}' --version-label '${{AWS_VERSION}}' "
    when:
      condition:
        all:
          masterBranch: "'${{CF_BRANCH}}' == '${{AWS_BRANCH}}'"
```

### Deployment Flow
- go to the Elastic Beanstalk service and create an application and environment
![codefresh](./images/codefresh_eb_environment.png)
- perform the following commands from root of your project
    * eb init
    * eb create ${{AWS_ENV_NAME}}

Note: If you don't have awsebcli - install EB CLI http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html

![codefresh](./images/codefresh_eb_health.png)

- add this repository to Codefresh, provide the necessary environments variables and build this service
![codefresh](./images/codefresh_eb_cf_step_deploy.png)