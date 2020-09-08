# Jenkins Devops Automation

Jenkins pipeline stages for Checkout from source, Build, Unit Testing, Sonarqube analysis, Run, Deployment. This jenkins pipeline is an complete example of Jenkins Devops Automation. 

## What is Jenkins
Jenkins – an open source automation server which enables developers around the world to reliably build, test, and deploy their software. The pipeline is a set of instructions given in the form of code for continuous delivery and consists of instructions needed for the entire build process. With pipeline, you can build, test, and deliver the application.

**Here are the reasons why you use should use Jenkins pipeline:**

Jenkins pipeline is implemented as a code which allows multiple users to edit and execute the pipeline process.
Pipelines are robust. So if your server undergoes an unforeseen restart, the pipeline will be automatically resumed.
You can pause the pipeline process and make it wait to resume until there is an input from the user.
Jenkins Pipelines support big projects. You can run multiple jobs, and even use pipelines in a loop.

_Pipeline Stages Examples:_

```sh
pipeline {
  stages {
    stage("Build") {
       steps {
          // Echo stage or text, Pipeline to the console
          echo "Building Application!"
          bat "C:/\"Program Files (x86)\"/MSBuild/14.0/Bin/MSBuild.exe C:/Windows/System32/config/systemprofile/AppData/Local/Jenkins.jenkins/workspace/Devops/GalaxyWebAPI.sln /t:Clean;Rebuild /p:Configuration=Release /property:OutDir=C:/Windows/System32/config/systemprofile/AppData/Local/Jenkins.jenkins/workspace/Devops/Temp/Publish /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER}"
       }
   }
   // And next stages if you want to define further...
    stage('SonarQube Analysis') {
       withSonarQubeEnv('QAEnv') {
          bat 'SonarScanner.MSBuild.exe begin /k:DevopsDev /v:1.0 /d:sonar.verbose=true /d:sonar.cs.opencover.reportsPaths=C:/\"Program Files (x86)\"/Jenkins/workspace/%JOB_BASE_NAME%/CodeCoverage/Printing_CodeCoverage.xml' 
          //bat 'C:/"Program Files (x86)"/MSBuild/14.0/Bin/MSBuild.exe C:/\"Program Files (x86)\"/Jenkins/workspace/%JOB_BASE_NAME%/GalaxyWebAPI.sln /t:Rebuild'
          bat 'C:/"Program Files (x86)"/"Microsoft Visual Studio"/2017/BuildTools/MSBuild/15.0/Bin/MSBuild.exe C:/\"Program Files (x86)\"/Jenkins/workspace/%JOB_BASE_NAME%/GalaxyWebAPI.sln /t:Rebuild'
          bat "SonarScanner.MSBuild.exe end"
        }
    }   
 } // End of stages
} // End of pipeline
```

