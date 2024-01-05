
pipeline {
    agent any
    stages
    {
        stage("test") {
            steps {
                bat './gradlew test'
                archiveArtifacts 'build/reports/tests/test/'
                cucumber buildStatus: 'UNSTABLE',
                         reportTitle: 'CucumberReport',
                         fileIncludePattern: '**/*.json',
                         trendsLimit: 10,
                         classifications: [
                               [
                                    'key': 'Browser',
                                    'value': 'Firefox'
                               ]
                         ]

            }
        }
        stage("Code Analysis"){
            steps{
                withSonarQubeEnv('sonar') {
                    bat "./gradlew sonar"
                }
            }
        }
        stage("Code Quality") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }

        stage("Build") {
            steps {
                bat './gradlew build'
                bat './gradlew javadoc'
                archiveArtifacts 'build/libs/*.jar'
                archiveArtifacts 'build/docs/'
            }
        }


        stage("Deploy & notification"){
            steps {
                bat './gradlew publish'
            }
            post {
                  failure {
                        notifyEvents message: 'Failure',
                        token: 'qn5ihe_o3vcuozbjdg-8ke-3lhiy3o6t'
                        mail to: 'ks_benni@esi.dz',
                        subject: "Failure",
                        body: "Something went wrong "
                  }
                  success {
                         notifyEvents message: 'Success ',
                         token: 'qn5ihe_o3vcuozbjdg-8ke-3lhiy3o6t'
                         mail to: 'ks_benni@esi.dz',
                         subject: "Success",
                         body: "Deployment successful"
                  }
            }
        }
    }
    post {
            always {
                junit 'build/reports/**/*.xml'
            }
    }
}
