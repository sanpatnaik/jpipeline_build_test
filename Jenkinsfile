node{

    checkout scm
    def mvnHome
    dir('BuildQuality'){
        stage('Preparation'){
                        
            git 'https://github.com/sanpatnaik/simple-spring.git'
            mvnHome = tool 'Maven'
        }

        stage('Build') {
            // Run the maven build
            if (isUnix()) {
                sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
            } else {
                bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }      
    }

        stage('Unit Test Results') {
            junit '**/target/surefire-reports/TEST-*.xml'
            //archive 'target/*.jar'
        }

    }


stage name:'Deploy to staging', concurrency:1
     
                //sh 'sudo docker run -d -p=3000:80 --network=bundlev2_prodnetwork nginx'
        dir('BuildQuality'){
        sh 'sudo docker-compose up -d --build'
    
                
}


   // def mvnHome
    dir('FunctionalTests'){

        stage('Get Functional Test Scripts'){                        
            git 'https://github.com/sanpatnaik/jenkins-selenium-int-testing.git'
            mvnHome = tool 'Maven'
        }

        stage('Run Tests') {
            sh "'${mvnHome}/bin/mvn' -Dgrid.server.url=http://seleniumhub:4444/wd/hub clean test "
        }
        stage('Functional Test Results') {
            junit '**/target/surefire-reports/TEST-*.xml'
        }
    }



stage name:'Shutdown staging'
     
                
        dir('BuildQuality'){
        sh 'sudo docker-compose stop'
    
                
}
}
