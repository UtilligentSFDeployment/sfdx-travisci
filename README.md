# Utilligent DX framework Package

This repository contains the Salesforce Metadata for the Utilligent DX framework Package.

## Installation

Install CLI - https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_install_cli.htm

Install VS Code - https://visualstudio.microsoft.com/downloads/

Download JDK & Set environment variable path- https://docs.oracle.com/en/java/javase/12/install/installation-jdk-microsoft-windows-platforms.html#GUID-371F38CC-248F-49EC-BB9C-C37FC89E52A0

Download GIT extensions - https://git-scm.com/

Download Nodejs - https://nodejs.org/en/download/

Download - https://rubyinstaller.org/downloads/ For travis CI ruby is required

## Dev Org Setup/ Enable Dev Hub
Refer Dev Org Setup- https://developer.salesforce.com/docs/atlas.en-us.externalidentityImplGuide.meta/externalidentityImplGuide/external_identity_create_developer_org.htm

Refer Dev Hub Setup- https://developer.salesforce.com/docs/atlas.en-us.216.0.sfdx_setup.meta/sfdx_setup/sfdx_setup_enable_devhub.htm

Can use below credentials to login (created by Aswin)
devorgdx@utilligent.com/utilligent2020

Authorize devhub
sfdx force:auth:web:login -d -a DevHub

sfdx force:auth:jwt:grant --clientid 3MVG97quAmFZJfVx8P9x_C4ywR.FkZx9OlYIpvFinZ9_y4vWWYa3s6yTUfCEUcSm_JP526NP0TyiEyhGNnVtH --username devorgdx@utilligent.com --jwtkeyfile D:/certificates/server.key -u devorgdx@utilligent.com

## GITHUB Account/ SSH setup
Refer https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account
Can use below credentials to login
UtilligentSFDeployment/utilligent2020
Paraphrase- utilligent

Set git username and password by following the steps -https://confluence.atlassian.com/bitbucket/configure-your-dvcs-username-for-commits-950301867.html

## Creating Self signed SSL certificate for non prod env
https://devcenter.heroku.com/articles/ssl-certificate-self
Open SSL certificate password - utilligent

## Scratch Org Setup

Refer - https://trailhead.salesforce.com/en/content/learn/projects/quick-start-salesforce-dx/create-and-test-our-scratch-org

1. Clone the repository and cd into the `MyTelstraDX` folder
```
git clone https://<d-number>@git02.ae.sda.corp.telstra.com/scm/sbc/b2c-mytelstra-dx.git
cd b2c-mytelstra-dx/MyTelstraDx
```

2. Authorize Dev Hub
```
sfdx force:auth:web:login -a DevHub -r https://telstra-retail.my.salesforce.com [-d]
```

3. Spin up a scratch org with provided scratch definition file
```
sfdx force:org:create -f config/template-scratch-def.json -a MyScratchOrg [-d 30] [-v DevHub]
```

## Push DX Source to Scratch Org
Skip this section if only validating

1. Push DX source to scratch Org
```
sfdx force:source:push -u MyScratchOrg [-f]
```

Push can only be used if the target env is a scratch org.
For sandbox, use deploy instead.
```
sfdx force:source:deploy -u MyScratchOrg
```

## Convert Metadata to DX Source
Only when needed

1. Convert Metadata to DX Source

Delete ../tmpSrc folder (if exists)
```
sfdx force:mdapi:convert -r ../src-DO-NOT-USE -d ../tmpSrc
```

## Validate DX Source in a Scratch Org
Only when needed

1. Convert DX Source to Metadata

Delete ../tmpMD folder (if exists)
```
sfdx force:source:convert -r force-app -d ../tmpMD
```

2. Validate if Metadata can be deployed to scratch Org
```
sfdx force:mdapi:deploy -d ../tmpMD -u MyScratchOrg -c
```

## Pull Changes from Scratch Org
Only when needed
```
sfdx force:source:pull -u MyScratchOrg [-f]
```

## Retrieve Metadata from Scratch Org
Only when needed
```
sfdx force:mdapi:retrieve -u MyScratchOrg -r ../retrieve -k ../package.xml
```

## Deploy Metadata to Scratch Org
Only when needed
```
sfdx force:mdapi:deploy -d ../tmpMD -u MyScratchOrg
```

## Export Data
Only when needed
Use Relational SOQL Queries if you need to export more than one related objects.
This SOQL exports all Accounts and their associated Contacts to JSON files.
These JSON files can be extracted once, added to repository and then imported to other Orgs.
```
sfdx force:data:tree:export -u MyScratchOrg -q "select Name, (select FirstName, LastName from Contacts) from Account" -p -d Data
```

## Import Data
Only when needed
```
sfdx force:data:tree:import -u MyScratchOrg -p Data\Account-Contact-plan.json
```

## Reference
https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev

https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference