# aks-azdevops-demo
Example project to setup CI/CD pipeline for AKS based on Azure DevOps

# Instructions
* Get AKS credentials
az aks get-credentials --resource-group demonstration --name kube-meetup

* Create new DevOps Project - kube-demo

* Create new git repo - backend

* Import code from github - https://github.com/Satyricon/simple-node-backend

* Setup Build pipeline
  * change pipeline file name to test-build-push-deploy-pipeline.yml
  * change tag from BuildId to BuildNumber

* Show automatically created Service Connections
  * Explain how Kube Service connection is done using Service Account

* Show automatically created Environment

* Rename pipeline to - Backend service - Test, build, push and deploy

* Check deployment from Environments, go to logs

* Check Container Registry

* Check created k8s Manifest files
  * pay attention that image in deployment.yml does not have tag

* Test deployment through browser (port 8080)

* Change service.yml port from 8080 to 80

* Add Testing Stage

```yaml
- stage: Test
  displayName: Test stage
  jobs:
  - job: Test
    displayName: Test
    pool:
      vmImage: $(vmImageName)
    steps:
    - task: Npm@1
      displayName: "NPM Install"
      inputs:
        command: 'install'

    - script: npm test
      displayName: "Run Tests"

    - task: PublishTestResults@2
      condition: succeededOrFailed()
      inputs:
        testRunner: JUnit
        testResultsFiles: '**/test-results.xml'
```

* Check Tests tab

* Fail the test

* Fix the code and update text to "Hello Kubernetes meetup"

* Add manual approval before deployment stage

* Create ARM Service Connection

* Create new Build pipeline based on release branch
  * give different name than azure-pipeline.yml - Backend CD pipeline
  * change tag from BuildId to BuildNumber
