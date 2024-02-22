## CICD for DOTNET APPLICATION DEPLOYED IN AZURE APP SERVICE
- First we need to create a organization in Github
- Assign the members to the organization with necessary permissions
- create a repo 
 ### Clone the repo into local machine using ssh
 ```
 git clone <ssh url>
 ```
- Copy source code to the cloned repo which is present in local machine
- Using the following commands to push the source code to github
 ```
 cd <repo name>
 git add .
 git status
 git commit -m "commit message"
 git push origin <branch name>
 ```
 ## Note 
 - while pushing the changes from local to github if we face issue as below
 ![GitHub Logo](/images/screenshot.png)
  ## Run the following commands
 ```
 git pull --rebase origin <branch name>
 git push origin <branch name>
 ```
## creating azure app service
 - Login to Azure cloud
 - Need to have subscription
 - Now go to azure app services and create a application according to the requirement
 - Download the publish profile for authentication with github
 - we have an option to add custom domain in app service.
 ## creating CICD
 - Create a workflow in the repo with this setup --.github/workflows/filename.yaml
 
 ### filename.yaml script
```
name: CI/CD for .NET
on:
  push:
    branches:
      - staging
  pull_request:
    branches:
      - '*'
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    if: github.event_name == 'pull_request'
    steps:
    - name: Checkout code
      uses: actions/checkout@v2
    - name: Setup .NET
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '6.0'
    - name: Restore dependencies
      run: dotnet restore
    - name: Build
      run: dotnet build
    - name: Test
      run: dotnet test
  deploy-staging:
    runs-on: ubuntu-latest
    #needs: build
    if: github.event_name == 'push' && github.ref == 'refs/heads/staging' && 'workflow_dispatch'
    steps:
    - name: Checkout code
      uses: actions/checkout@v2
    - name: Setup .NET
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '6.0'
    - name: Publish
      run: dotnet publish -c Release -o ./publish
    - name: Deploy to Azure Web App
      uses: azure/webapps-deploy@v2
      with:
        app-name: zelar12
        #slot-name: <slot-name>
        publish-profile: ${{ secrets.AZUREAPPSERVICE_PUBLISH_PROFILE_STAGING }}
        package: ./publish
  deploy-Release:
    runs-on: ubuntu-latest
    needs: deploy-staging
    #if: github.event_name == 'push' && github.ref == 'refs/heads/Release' && 'workflow_dispatch'
    steps:
    - uses: trstringer/manual-approval@v1
      with:
       secret: ${{ secrets.GITHUBTOKEN }}
       approvers: rinizelar
       minimum-approvals: 1
       issue-title: "Deploying v9.0.0 from feature to main"
       issue-body: "Please approve or deny the deployment of version v9.0.0"
       exclude-workflow-initiator-as-approver: false
       additional-approved-words: ''
       additional-denied-words: ''
    - name: Checkout code
      uses: actions/checkout@v2
    - name: Setup .NET
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '6.0'
    - name: Publish
      run: dotnet publish -c Release -o ./publish
    - name: Deploy to Azure Web App
      uses: azure/webapps-deploy@v2
      with:
        app-name: zelar11
        #slot-name: <slot-name>
        publish-profile: ${{ secrets.AZUREAPPSERVICE_PUBLISH_PROFILE }}
        package: ./publish	    
```
### Note:
 - If Pull request is raised then CI will run everytime
 - Here we created manual approval when the code is merged to production it will ask for approval
 - Add the app name and secret variable in yaml file
 - we need to add secrect of azure app service in github secrets.
