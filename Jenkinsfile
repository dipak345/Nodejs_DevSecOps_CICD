pipeline{
    agent any
    
    environment{
        SCANNER_HOME= tool 'Sonar'
    }
    stages{
        stage('checkout'){
            steps{
                git branch: 'master', url: 'https://github.com/dipak345/Nodejs_DevSecOps_CICD.git' 
            }
        }
        
          stage('Sonar Analysis') {
            steps {
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url==http://54.87.213.171:9000/ -Dsonar.login=squ_7f8f759a165699433daf165cee6ac2128762474a -Dsonar.projectName=to-do-app \
                   -Dsonar.sources=. \
                   -Dsonar.projectKey=to-do-app '''
               }
            }
        //stage('OWASP'){
        //    steps{
        //       dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'OWASP'
        //        dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        //    }
        //}
        stage('docker-build'){
            steps{
                sh "docker build -t nodejs_app ."
            }
        }
        stage("Trivy"){
            steps{
                sh "trivy image nodejs_app"
            }
        }
        
        stage('docker-push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'ndj', passwordVariable: 'pwd', usernameVariable: 'uname')]) {
                sh "docker tag nodejs_app ${env.uname}/nodejs_app:latest"
                sh "docker login -u ${env.uname} -p ${env.pwd}"
                sh "docker push ${env.uname}/nodejs_app:latest"
        }
            }
        }
        stage('docker-deploy'){
            steps{
                sh "docker run -itd -p 8000:8000 dipakkhilare567/nodejs_app:latest"
                echo "App Deployed Successfully"
            }
        }
       
    }
} 
