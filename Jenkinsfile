pipeline
{
agent any

tools
{
    jdk 'jdk-21'
    maven 'mymaven'
}



stages{

    stage('compile'){
        steps{
            sh "mvn clean compile"
        }
    }

    stage(sonar_analysis){
        steps{
            sh'''
             mvn sonar:sonar \
            -Dsonar.host.url=http://35.173.228.159:9000 \
            -Dsonar.login=squ_e325604b23840e3fbdf607ee2525239c0b195b10\
            -Dsonar.projectKey=shopping_cart \
            -Dsonar.projectName=shopping_cart \
            -Dsonar.java.binaries=.
            '''

        }
    }

    stage('OWASP'){
        steps{

           withCredentials([string(credentialsId: 'nvd_api', variable: 'nvd_api_key')]) {

            dependencyCheck(
             additionalArguments: "--scan ./ --format XML --nvdApiKey ${nvd_api_key}",
             odcInstallation: 'dp-check'
    )
}
            
        }
    }

    stage('build'){
        steps{
            sh 'mvn clean install'
        }

    }

    stage('Docker build&push'){
        steps{
            script{
                  withDockerRegistry(credentialsId: 'dockerhub-cred', toolName: 'docker') {
               sh "docker build -t shopping:latest -f docker/Dockerfile"
               sh "docker tag shopping:latest vpk1999/shopping:latest"
               sh "docker push vpk1999/shopping:latest"
               
               }
            }
        }
    }
}
}