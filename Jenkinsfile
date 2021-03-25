pipeline{
      agent any
      triggers{
          bitbucketPush()
            }

// jenkins app passwd jv8ZVrXMpyAqkx3EAPav      
      
      stages{
              //code scanning with specific branch scanning
          stage("build"){
              environment {
                    scannerHome = tool 'SonarQube Scanner 4.3.0.2102'
                    ORGANIZATION = "rj-raghu"
                    }
          when {
            branch 'master'
                }
        steps {
            withSonarQubeEnv('scanner') {
            sh "${scannerHome}/bin/sonar-scanner \
           -Dsonar.projectKey=test-node-js \
           -Dsonar.sources=. \
           -Dsonar.css.node=. \
           -Dsonar.host.url=https://sonarcloud.io/ \
           -Dsonar.organization=$ORGANIZATION \
           -Dsonar.login=a718bda0c0745effc8ec3853cd4da70c7c552afa"
           
            }
        timeout(time: 7, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
            }
        
          }
              
        
    }
}
}
