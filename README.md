# Power BI Projects Workflow

## What is this?  

This repository is a example how to work in collaboration with Power BI Projects using Git and GitHub Workflow resolving conflicts and building a parallel project sample. 
It demonstrates the use of a CI/CD scenario for Microsoft Power BI PRO projects by utilizing [fabric-cli](https://aka.ms/fabric-cli) and GitHub Actions.  


## Quick Links  

- Video demo of the reference repo: https://youtu.be/sgVqrOUgXro  
- How to get Service Principal: https://youtu.be/IFp1Aingnmw  
- Learn Git and GitHub for MS Fabric and Power BI: https://youtu.be/wFimCGpndOc  
- Video for conceptual reference for automated testing (technical methodology has changed): https://youtu.be/PL7Xw2dvVrE


## Instalation 

- Create a copy of `.env.example` and rename to `.env`  and fill with the SPN secrets    
- Configure the same SPN secrets on GitHub Actions if you want CD


|Name|Value|  
|---|---|  
|FABRIC_CLIENT_ID|Your Service Principal Client ID from Entra App ID|  
|FABRIC_CLIENT_SECRET|Your Service Principal Secret from Entra App ID|  
|FABRIC_TENANT_ID|Your Tenant ID|  


If you run locally, in the first time install the requirements with:    

```bash
$ pip install -r requirements.txt  
```

- Configure  the file `config.json` with ids, names by branch.  
  - The  adminUPNs are the Object ID of the Entra User Principal Name.  


## Automated tests
- Specify all required functions and table references in the env step of the M code at:
src\Project.SemanticModel\definition\tables\aux_testes.tmdl
- Define your test cases — including the M expression and expected result — in the testCases step of the same file.


Accurate specification of dependencies is critical for the correct interpretation of M language scripts within the Power BI editor. Incorrect or incomplete dependency declarations can lead to failures in automated testing processes.


## CI/CD Pipeline  

- The Power BI Projects are saved in the `src` folder with the extensions `*.Report` and `*.SemanticModel`.  
- Deploying new versions is done on the `dev` branch.  
- On every Pull Request to `develop`, the GitHub Actions pipeline called `.github\workflows\bpa.yml` is triggered, running the best practices analysis pipeline. This process utilizes community tools such as [Tabular Editor](https://github.com/TabularEditor/) and [PBI-Inspector](https://github.com/NatVanG/PBI-InspectorV2). If approved and merged the pipeline `.github\workflows\deploy.yml`  will deploy to the workspace `*-DEV` workspace with the name and data source specified in `config.json`.  
 - Once the project is approved, a pull request is created for the `main` branch, where the pipeline will deploy the version to the workspace `*-PRD` workspace following the same `config.json`.

