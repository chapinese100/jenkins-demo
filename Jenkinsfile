pipeline {
    agent any 
    stages {
        stage('Clone') {
            steps {
                echo "Repository Parsing"
                git 'https://github.com/vathsalahn/jenkins-demo.git'
            
            }
        }
        stage('build'){
            steps {
                echo "Project Building"
                sh "cd MavenProject ; mvn clean install ; pwd"
            }
        }
        
        stage('Archiving Artifacts'){
            steps {
                echo "archiving the artifacts"
                archiveArtifacts 'MavenProject/multi3/target/*.war'
            }
            
        }
        stage('Deployment Project') {
            // Deployment
            steps {
                script {
                    echo "deployment"
                    sh 'cp MavenProject/multi3/target/*.war /Applications/apache-tomcat-7.0.88/webapps/'
                }
            }
        }
        stage('HTML Report Generation') {
            steps{
                echo "publishing the html report"
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
            }
        }
        stage('Cleaning Up') {
            steps {
                echo "cleaning up the workspace"
                cleanWs()
            }
        }
        stage("Metrics"){
            steps{
            parallel ( "JavaNcss Report":   
            {
              node('window'){
                git 'https://github.com/vathsalahn/jenkins-demo.git'
                sh "cd javancss-master ; mvn test javancss:report ; pwd"
                  }
            },
            "FindBugs Report" : {
            node('window'){
                sh "mkdir javancss1 ; cd javancss1 ;pwd"
                git 'https://github.com/vathsalahn/jenkins-demo.git'
                sh "cd javancss-master ; mvn findbugs:findbugs ; pwd"
                deleteDir()
                }

              }
         )
            }
         post{
                success {
                    emailext body: 'Successfully completed pipeline project with archiving the artifacts', subject: 'Pipeline was successfull', to: 'vathsala.hn22@gmail.com'
                }
    }
}
        
    }
}
