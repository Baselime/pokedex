// make sure to add "baselime-api-key" to your Jenkins credentials prior to running this pipeline

pipeline {
    agent any

    stages {

        stage("Plan") {

            agent {
                docker {
                    image 'baselime/baselime:latest'
                    args '--entrypoint='
                    reuseNode true
                }
            }

            environment {
                BASELIME_API_KEY = credentials('baselime-api-key')
            }

            steps {
                sh 'baselime plan --c .baselime'
            }
        }

        stage("Push") {

            agent {
                docker {
                    image 'baselime/baselime:0.0.30'
                    args '--entrypoint='
                    reuseNode true
                }
            }

            environment {
                BASELIME_API_KEY = credentials('baselime-api-key')
            }

            input {
                message "Do you want to push the changes?"
                ok "Yes"
            }

            steps {
                // this step will push the changes without asking for confirmation again
                sh 'baselime push -y --c .baselime'
            }
        }
    }
}