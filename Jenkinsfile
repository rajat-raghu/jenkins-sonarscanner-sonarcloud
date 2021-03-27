node {
    timestamps {
        String databricks_token = ""

        stage('initialize') {
            def gitRepo = checkout scm
            // String git_url = gitRepo.GIT_URL
            git_branch = gitRepo.GIT_BRANCH.tokenize('/')[-1]

            String git_commit_id = gitRepo.GIT_COMMIT
            String git_short_commit_id = "${git_commit_id[0..6]}"


            String buildTime = sh(returnStdout: true, script: "date +'%Y.%V'").trim()
            currentBuild.displayName = ("${databricks_environment}" + "." + git_branch + "." + buildTime + "." + currentBuild.number)

            if(!databricks_environment) {
                error("databricks environment is empty or not defined.")
            }

            if(databricks_environment == "prod") {
               databricks_token = "rx-prod-databricks-token"
            } else if (databricks_environment == "stage") {
                 databricks_token = "rx-stage-databricks-token"
            }else if (databricks_environment == "dev"){
                 databricks_token = "rx-dev-databricks-token"
            }

            // withCredentials([string(credentialsId: databricks_token, variable: 'TOKEN')]) {
            //     databricks_bearer_token = TOKEN
            // }

            if(!databricks_token) {
                error("databricks_token is empty or not defined.")
            }

        }

        stage('Deploy the notebooks') {
              if(env.DeployNotebook.toBoolean()){
                

                       sh """
                       echo "Deploy with nOTEBOOK"
                       #export DATABRICKS_HOST=https://dbc-31fd7737-02c1.cloud.databricks.com
                       #export DATABRICKS_TOKEN=${databricks_bearer_token}

                       #/usr/local/bin/databricks workspace mkdirs /Users/rx.etl.${databricks_environment}@utopusinsights.com/etl-measurement-data/etl-asset-master-data-to-fast-store/
                       #/usr/local/bin/databricks workspace import_dir -o -e ${WORKSPACE} /Users/rx.etl.${databricks_environment}@utopusinsights.com/etl-measurement-data/etl-asset-master-data-to-fast-store/

                       """
                
                }
    		}

        stage('Create the job') {
             if(env.CreateJob.toBoolean()){

                       sh """
                       echo "cREATE THE JON"
                       #export DATABRICKS_HOST=https://dbc-31fd7737-02c1.cloud.databricks.com
                       #export DATABRICKS_TOKEN=${databricks_bearer_token}

                       #/usr/local/bin/databricks jobs create --json-file ${WORKSPACE}/deployment_files/${databricks_environment}-job-config.json

                       """
              }
              else{echo "SKIPPED CREATION OF JOB"}
    		}

        stage('Reset the job') {
              if(env.UpdateJob.toBoolean()){

                       sh """
                       echo "Reset the job"
                       #export DATABRICKS_HOST=https://dbc-31fd7737-02c1.cloud.databricks.com
                       #export DATABRICKS_TOKEN=${databricks_bearer_token}

                       #/usr/local/bin/databricks jobs reset --job-id ${JOB_ID} --json-file ${WORKSPACE}/deployment_files/${databricks_environment}-job-config.json

                       """
              }
              else{echo "NOT RESETTING THE JOB"}
    		}

   }
}
