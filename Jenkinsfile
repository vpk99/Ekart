pipeline
{
agent any

tools
{
    java 'jdk'
    maven 'mymaven'
}

environment
{
    SCANNER_HOME= tool 'sonar-scan'
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
            $SCANNER_HOME/bin/sonar-scan -Dsonar.url=http://54.175.243.242:9000/ -Dsonar.login=squ_a99bf2bb78e79b01e5e71f49c86f9b1bef163fc1\
            -Dsonar.projectName=shopping_cart\
            -Dsonar.java.binaries=.\
            -Dsonar.projectkey=shopping_cart
            '''

        }
    }
}
}