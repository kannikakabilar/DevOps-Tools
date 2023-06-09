# GitHub-Actions
This repo is used to design various GitHub Action workflows. Go to .github/workflows to view the all workflows.

##### build the project

    ./gradlew build

##### build Docker image called java-app. Execute from root

    docker build -t java-app .
    
##### push image to repo 

    docker tag java-app demo-app:java-1.0
