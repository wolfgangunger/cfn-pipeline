# Codepipeline for automated CFN Deployments
Automation for cloudformation deployments with code pipeline


  
Example Project for Cloudformation Repo
<a href="https://github.com/wolfgangunger/cfn-for-pipeline" target="_blank">CFN Example Repo</a>   
  

   
## introduction
this project contains a codepipeline that will allow automated cloudformation deployment with codepipeline  
  
it is setup for a one account scenario - multi account deployments from a toolchain account to multiple  
stage accounts would be possible, but complicate this project   
therefore, if you have 3 stage accounts (dev, qa, prod), setup the pipeline in each account.  

Architecture:
![image](https://github.com/wolfgangunger/cfn-pipeline/blob/main/architecture-cfn-pipeline.jpg)

## setup
you need a repo with your cloudformation templates you want to deploy - see example repo above  
follow the naming conventations of folders and also for the template yaml files and parameter json files  
include the python script 'create_pipelines.py' in your cloudformation repo in a folder names 'scripts'. See example repo project.  

You need to setup a codestar connection to allow codepipeline to read from this repo - it can be github, bitbucket and gitlab.   
Next deploy the template 'cfn-pipeline-generator' in the account and region where you want to deploy the cloudformation templates by pipeline.    
it will deploy a pipeline template first, this pipeline is not runnable, it just serves as template for creating the other pipelines for your cloudformation stacks  
This template pipeline is stetup with 'trigger on push = false' , so the cloudformatin deployment pipelines will not 
start automatically once you make changes in your cfn-repo. 
you have to trigger the dedicated pipeline for each stack if you want to deploy to dev, qa or prod. this is designed on purpose like this.  



Next it will create a pipeline for each folder in your cfn-repo  
please see my git repo 
[Repo CFN-Example](https://github.com/wolfgangunger/cfn-for-pipeline)
 as example :
structure must be:   
cfn_[foldername]  
-template.yaml  
-params_[stage].json  

example :   
cfn_001_simple_user  
-template.yaml  
-params_dev.json  
-params_qa.json  
-params_prod.json  



you will see as many pipelines as you have subfolders (cfn_[foldername]) with templates in your cfn-repo  
this will look like this:  
![image](https://github.com/wolfgangunger/cdk-cfn-pipeline/blob/main/pipeline-cfn2.jpg)


you can now use these pipelines to deploy to your stage account(s) and avoid manual deployments on the cfn web-console  

if you are adding new templates to your github repo, run again the pipeline generator, it will create new pipelines for new templates  
if you want to update a existing template, commit the changes to the repo and run the associated pipeline for this stack, it will perform  
a update Stack operation for an existing Stack.  

More Infos this Project in my Blog :  
<a href="https://www.sccbrasil.com/blog/aws/cfn-structure.html" target="_blank">Automated CloudFormation Deplyoment with CodePipeine</a>  

See more Infos about structuring and layering CFN Templates :  
<a href="https://www.sccbrasil.com/blog/aws/cfn-structure.html" target="_blank">Structuring CloudFormation</a>  


Same Project with Automated Pipeline in CDK  
<a href="https://github.com/wolfgangunger/cdk-cfn-pipeline" target="_blank">CDK Pipeline for CFN Deployments</a>   