
pipeline {
    agent any
    {
        stage("test") {
            steps {
                bat './gradlew test' archiveArtifacts 'build/test-results/test/'
                cucumber buildStatus: 'UNSTABLE',
                reportTitle: 'CucumberReport',
                fileIncludePattern: '**/*.json',
                trendsLimit: 10,
                classifications:
                [['key': 'Browser', 'value': 'Edge']]
            }
        }
    }
}
