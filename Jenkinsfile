/*
 * Created by Gaurav Yadav
*/

pipeline{
  agent any
  //discard after number of specified builds
  options { buildDiscarder(logRotator(numToKeepStr: "5")) }
  parameters {
        choice(
          name: 'environment',
          choices: ['dev','qa','stage', 'production'],
          description: 'Environment for this build'
          )
    }

  environment {
    dev_creds = "*********"
  }
  stages{

    stage("Deploy on Dev"){
      when{
        expression {
          return ( 
          ((params.environment == "dev"))
          )
        }
      }

      steps{
        timeout(time: 2400, unit: 'SECONDS'){
            echo "Deploy Aurora PostgreSQL Cluster on Dev Env"
            withAWS(role:'iam-role', roleAccount:'***********', roleSessionName: 'CFN-dev-deployment-session', region:'eu-west-1') {
            cfnUpdate(stack:"dev-aurora-postgresql-cfn", file:"aurora-postgresql-data-anomalies/deployments/aurora-postgresql-data-anomalies.json", paramsFile:"aurora-postgresql-data-anomalies/deployments/parameter.${params.environment}.json", timeoutInMinutes:40)  
          }
      }
     }
    }

    stage("Deploy on QA"){
      when{
        expression {
          return ( 
          ((params.environment == "qa"))
          )
        }
      }

      steps{
        timeout(time: 2400, unit: 'SECONDS'){
            echo "Deploy Aurora PostgreSQL Cluster on QA Env"
            withAWS(role:'iam-role', roleAccount:'***********', roleSessionName: 'CFN-qa-deployment-session', region:'eu-west-1') {
            cfnUpdate(stack:"qa-aurora-postgresql-cfn", file:"aurora-postgresql-data-anomalies/deployments/aurora-postgresql-data-anomalies.json", paramsFile:"aurora-postgresql-data-anomalies/deployments/parameter.${params.environment}.json", timeoutInMinutes:40)
          }
      }
     }
    }


    stage("Deploy on Stage"){
      when{
        expression {
          return ( 
          ((params.environment == "stage"))
          )
        }
      }

      steps{
        timeout(time: 2400, unit: 'SECONDS'){
        echo "Deploy Aurora PostgreSQL Cluster on Stage Env"
        withAWS(role:'iam-role', roleAccount:'***********', roleSessionName: 'CFN-stage-deployment-session', region:'eu-west-1') {
        cfnUpdate(stack:"stage-aurora-postgresql-cfn", file:"aurora-postgresql-data-anomalies/deployments/aurora-postgresql-data-anomalies.json", paramsFile:"aurora-postgresql-data-anomalies/deployments/parameter.${params.environment}.json", timeoutInMinutes:40)
          }
      }
      }
    }

        stage("Deploy on production"){
      when{
        expression {
          return ( 
          ((params.environment == "prod"))
          )
        }
      }

      steps{
        timeout(time: 2400, unit: 'SECONDS'){
        echo "Deploy Aurora PostgreSQL Cluster on Prod Env"
        withAWS(role:'iam-role', roleAccount:'***********', roleSessionName: 'CFN-dev-deployment-session', region:'eu-west-1') {
        cfnUpdate(stack:"prod-aurora-postgresql-cfn", file:"aurora-postgresql-data-anomalies/deployments/aurora-postgresql-data-anomalies.json", paramsFile:"aurora-postgresql-data-anomalies/deployments/parameter.${params.environment}.json", timeoutInMinutes:40)
          }
      }
      }
    }
    }

}
