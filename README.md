# aks-azdevops-demo
Example project to setup CI/CD pipeline for AKS based on Azure DevOps

# Instructions
* Get AKS credentials
az aks get-credentials --resource-group demonstration --name kube-demo

* Create new DevOps Project - kube-demo

* Create new git repo - backend

* Import code from github - https://github.com/Satyricon/simple-node-backend

* Setup Environments

* Setup Build pipeline
    * change environment to development.dev
    * change tag from BuildId to BuildNumber
    * rename pipeline to - Backend development - Build, push and deploy service

* check created k8s Manifest files
    * pay attention that image in deployment.yml does not have tag

* change service.yml port from 8080 to 80

* check deployment from Environments, go to logs

* create develop branch and change pipeline to be based on develop branch

* create new Build pipeline based on master branch
    * give different name than azure-pipeline-1.yml
    * change tag from BuildId to BuildNumber

* Add Testing Stage
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