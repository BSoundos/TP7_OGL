
pipeline {
    agent any
    stages
    {
        stage("test") {
            steps {
                bat './gradlew test'
                archiveArtifacts 'build/test-results/test/'
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
    }
}
