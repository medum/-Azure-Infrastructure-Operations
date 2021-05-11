Deploying a Web Server in Azure
            
     Project Setup
        Create an Azure Account
        Install the Azure command line interface
        Install Packer
        Install Terraform
        
  Deploy a Policy:
      
    Policy Definition: policy that ensure that all indexed resources in the subscription that have tags and deny deployment if they do not.
    
  Steps to Implement:
     
     Step 1: Log in to portal "https://portal.azure.com/"
     Step 2: Click on the the icon
     Step 3: Clone the repository using gitclone <http repository link>
     Step 4: Log in to Cli "az login"
     Step 5: Execute below commands 
              az policy definition create --name TagsPolicyDefinition --mode All --rules azurepolicy.json --params azurepolicy.parameters.json
output:
![image](https://user-images.githubusercontent.com/49653011/117850904-c27e0300-b253-11eb-8107-b8c1c14efbd0.png)

              az policy definition show --name TagsPolicyDefinition
output:
![image](https://user-images.githubusercontent.com/49653011/117851263-23a5d680-b254-11eb-8c3e-b9b98fd52447.png)
![image](https://user-images.githubusercontent.com/49653011/117851328-31f3f280-b254-11eb-9767-5cf43e207c65.png)

              az policy assignment create --name TagsPolicyDefinition --policy TagsPolicyDefinition --param  azurepolicy.paramasignment.json
output:
![image](https://user-images.githubusercontent.com/49653011/117851432-50f28480-b254-11eb-9beb-7dd334a34177.png)
             az policy assignment create --name tagging-policy --policy TagsPolicyDefinition --param azurepolicy.paramasignment.json
  output:
  ![image](https://user-images.githubusercontent.com/49653011/117851593-767f8e00-b254-11eb-8106-dde2dd6dc766.png)

    Step 6: az policy assignment list
output:
    
![image](https://user-images.githubusercontent.com/49653011/117851769-a2027880-b254-11eb-8c81-7bc46834fcdd.png)
![image](https://user-images.githubusercontent.com/49653011/117851813-af1f6780-b254-11eb-803d-f3e2b83d564c.png)
![image](https://user-images.githubusercontent.com/49653011/117852039-f0177c00-b254-11eb-9108-72b0c126f96e.png)
![image](https://user-images.githubusercontent.com/49653011/117852110-01608880-b255-11eb-9c5d-ef546df72d64.png)


   Packer:
   
    Problem Statement :create a server image, ensuring that the provided application is included in the template.
    
  Steps to Implement:
  
     Step1:  Create a resource group for your image. It will be used as a variable with default value into packer file "server.json" ( example: demo-rg), also will be used in terraform
              This can be done using portal or cli
              Using portal follow below steps
            https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal
              Using Cli execute below command:
                az group create -l centralus -n <name of the group>
    Step2: Complete server.json
    Step3: Execute below command :
          packer build Packer/server.json
    Step4: out out should look like this":
 ![Capture](https://user-images.githubusercontent.com/49653011/117557919-17e4c500-b046-11eb-9be8-cd72387b99c5.PNG)


Terraform
 Problem Statement: Terraform template should allow to reliably create, update, and destroy our application. Template should use variables and loops, along with deploy a web app that has been loaded into our Packer template already.
 Steps to Implement:
  
     Step1: Log in to Azure portal, click on the icon ![image](https://user-images.githubusercontent.com/49653011/117706568-77a0b480-b19b-11eb-939e-0f4c1817aa1a.png)
            it will load the command promt. Create project structure.
      Create below files:
            main.tf : Main code
            vars.tf : Will hold vriables	
            locals.tf:	Security rules are defined here
            terraform.tfvars:	used to provide default variable values
     Step2: 
     To get subscription id execute below command on command prompt
     az account show --query "{ subscription_id: id }"
     To get client_id, client_secret, tenant_id execute below command:
     az ad sp create-for-rbac --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"
     Step3:
      Go the folder where the files are created 
      Execute below command 
      terraform init
      Output: Sucess
  ![image](https://user-images.githubusercontent.com/49653011/117708061-5b9e1280-b19d-11eb-84f7-a28becbfe170.png)

     Step 4:
      Execute below command to validate the code :
      terraform validate
      output : Sucess
    
    Step 5:
     To create the plan ececute below command:
     terraform plan -out solution.plan    
     Output should look like this:
  ![image](https://user-images.githubusercontent.com/49653011/117705471-11676200-b19a-11eb-8761-e8f16a010aa5.png)
  
    Step 6:
       To create the infrastructure execute the below command:
       terraform apply solution.plan
       Output should look like this
 ![image](https://user-images.githubusercontent.com/49653011/117719431-62cc1d00-b1ab-11eb-9d2f-bd3f7c1d380c.png)
     Step 7:
       To destroy please execute below commnds:
       
         terraform plan -destroy -out solution.destroy.plan
         terraform apply solution.destroy.plan
       
![image](https://user-images.githubusercontent.com/49653011/117723457-64e4aa80-b1b0-11eb-99d2-e1098247b492.png)

    
Note:If got any errors while applying the solution, then delete all files that got created by the commands and repea all the steps above
