node{

    checkout scm
    dir('BuildQuality'){
        stage('Preparation'){
          git 'https://github.com/sanpatnaik/simple-spring.git'
        }
        stage('Unit Test Results') {
            junit '**/target/surefire-reports/TEST-*.xml'
            //archive 'target/*.jar'
        }

    }
}

stage name:'Deploy to staging', concurrency:1
    node {
                //sh 'sudo docker run -d -p=3000:80 --network=bundlev2_prodnetwork nginx'
        dir('BuildQuality'){
        sh 'sudo docker-compose up -d --build'
    }
                
}

node{
    def mvnHome
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

}

stage name:'Shutdown staging'
    node {
                
        dir('BuildQuality'){
        sh 'sudo docker-compose stop'
    }
                
}
