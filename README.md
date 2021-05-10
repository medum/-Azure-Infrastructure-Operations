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
              az policy assignment create --policy TagsPolicyDefinition --params azurepolicy.paramasignment.json
              az policy assignment create --name 'Tagging-Policy-Assignment' --params azurepolicy.paramasignment.json
    Step 6: out put when you execute "az policy assignmentlist"
          ![az-policyAssignmentlist](https://user-images.githubusercontent.com/49653011/117557678-a4da4f00-b043-11eb-87cb-0263a4f60cd8.PNG)

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
     
     
     
     ![image](https://user-images.githubusercontent.com/49653011/117705471-11676200-b19a-11eb-8761-e8f16a010aa5.png)

