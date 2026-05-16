pipeline
{
agent any

tools
{
    jdk 'jdk'
    maven 'mymaven'
}



stages{

    stage('build'){
        steps{
            sh "mvn clean compile"
        }
    }

    stage(sonar_analysis){
        steps{
            sh'''
             mvn sonar:sonar \
            -Dsonar.host.url=http://3.84.217.18:9000 \
            -Dsonar.login= squ_e325604b23840e3fbdf607ee2525239c0b195b10\
            -Dsonar.projectKey=shopping_cart \
            -Dsonar.projectName=shopping_cart \
            -Dsonar.java.binaries=.
            '''

        }
    }
}
}