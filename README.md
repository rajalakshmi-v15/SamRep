# SamRep
Sample repository to check Pierre de-check (https://github.com/alvarocavalcanti/pierre-decheck)

A Git webhook has been attached to the repo to monitor dependencies.


Last line has been added.

```
pipeline {
    agent any
    stages {
        stage('Initialize') {
            steps {
                build job: 'init_pipeline', parameters: [string(name: 'folder_name', value: "$FOLDER_NAME")]
            }
        }
        stage('Plan VPC'){
            steps {
                build job: 'vpc', parameters: [string(name: 'cidr_block', value: '10.0.0.0/16'), string(name: 'name', value: "$VPC_NAME"), string(name: 'owner', value: "$OWNER"), string(name: 'az_code', value: 'b'), string(name: 'folder_name', value: "$FOLDER_NAME"), string(name: 'project', value:"$PROJECT")]
            }
        }
        stage('Create VPC'){
            steps{
                build job: 'apply_changes', parameters: [string(name: 'folder_name', value: "$FOLDER_NAME")]
            }
        }

        stage('Plan subnets'){
            steps{
                build job: 'subnet_create', parameters: [string(name: 'cidr_block', value: '10.0.1.0/24'), string(name: 'name', value: 'test_public_subnet'), string(name: 'owner', value: "$OWNER"), string(name: 'folder_name', value: "$FOLDER_NAME"), string(name: 'project', value: "$PROJECT"), string(name: 'vpc_name', value: "$VPC_NAME"), string(name: 'public', value: 'true')]
                build job: 'subnet_create', parameters: [string(name: 'cidr_block', value: '10.0.2.0/24'), string(name: 'name', value: 'def_parser_test_private_subnet'), string(name: 'owner', value: "$OWNER"), string(name: 'folder_name', value: "$FOLDER_NAME"), string(name: 'project', value: "$PROJECT"), string(name: 'vpc_name', value: "$VPC_NAME"), string(name: 'public', value: 'false')]
            }
        }
        stage('Create subnets'){
            steps{
                build job: 'apply_changes', parameters: [string(name: 'folder_name', value: "$FOLDER_NAME")]
            }
        }
```


