pipeline
{
agent {label 'dev-server'}

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
            -Dsonar.host.url=http://23.20.113.82:9000 \
            -Dsonar.login=squ_e325604b23840e3fbdf607ee2525239c0b195b10\
            -Dsonar.projectKey=shopping_cart \
            -Dsonar.projectName=shopping_cart \
            -Dsonar.java.binaries=.
            '''

        }
    }

    stage('OWSAP'){
        steps{
             dependencyCheck additionalArguments: '--scan ./ --format XML',
             odcInstallation: 'dp'

            dependencyCheckPublisher pattern: '**/dp-report.xml'
            
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
                  withDockerRegistry(credentialsId: 'dockerhub-creds', toolName: 'docker') {
               sh "docker build -t shopping:latest -f docker/Dockerfile"
               sh "docker tag shopping:latest vpk1999/shopping:latest"
               sh "docker push vpk1999/shopping:latest"
               
               }
            }
        }
    }
}
}