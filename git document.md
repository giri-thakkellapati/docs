# CICD for DOTNET APPLICATION

 ### First we need to create a organization in Github
 ### Assign the members to the organization with necessary permissions
 ### create a repo 
 ### Clone the repo into local machine using ssh
 ```
 git clone <ssh url>
 ```
 ### Copy source code to the cloned repo which is present in local machine
 ### Using the following commands to push the source code to github
 ```
 cd <repo name>
 git add .
 ```
 ```
 git status
 ```
 git commit -m "commit message"
 ```
 ```
 git push origin <branch name>
 ```
 ## Note 
 while pushing the changes from local to github if we face issue as below
 
 /home/giri/Pictures/Screenshots/Screenshot from 2024-02-22 11-21-38.png
 
 ```
 git pull --rebase origin <branch name>
 git push origin <branch name>
 ```
 
 
 ## Run the following commands
 
 ## creating CICD
 ###Create a workflow in the repo with this setup .github/workflows/filename.yaml
 
 ### filename.yaml script



```
	name: CI/CD for .NET

	on:
	  push:
	    branches:
	      - main
	  pull_request:
	    branches:
	      - '*'

	jobs:
	  CI:
	    runs-on: ubuntu-latest
	    if: github.event_name == 'pull'
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
	      
	  CD:
	    runs-on: ubuntu-latest
	    #needs: build
	    if: github.event_name == 'push' && github.ref == 'refs/heads/main'
	    
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
		app-name: zelar11
		publish-profile: ${{ secrets.AZUREAPPSERVICE_PUBLISH_PROFILE }}
		package: ./publish
```
 
 
 Add the app name and secret in yaml file
 we need to add screct for azure app service in github in secrets. 
 
 ### Note 
 
  If Pull request is raised then CI will run
  Once the PR is merged to main/master branch then CD will run and application will be deployed in azure app service
