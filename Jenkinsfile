pipeline {
    agent any
    tools {
        maven 'mymaven1'
    }
    parameters {
        string(name: 'Env', defaultValue: 'Test', description: 'Version to deploy')
        booleanParam(name: 'executeTests', defaultValue: true, description: 'Decide to run test cases')
        choice(name: 'APPVERSION', choices: ['1.1', '1.2', '1.3'], description: 'Select application version')
    }

    stages {
        stage('Compile') {
            steps {
                script {
                    echo 'Compiling the code'
                    echo "Compiling the code for ${params.Env} environment"
                    sh 'mvn compile'
                }
            }
        }
        stage('CodeReview') {
            steps {
                script {
                    echo 'Reviewing the code'
                    sh 'mvn pmd:pmd'
                }
            }
        }
        stage('UnitTest') {
            when {
                expression {
                    params.executeTests==true
                }
            }
            steps {
                script {
                    echo 'Testing the code'
                    sh 'mvn test'
                }
            }
        }
        stage('CoverageAnalysis') {
            steps {
                script {
                    echo 'Analysing the code'
                    sh 'mvn verify'
                }
            }
        }
        stage('Package') {
            steps {
                script {
                    echo 'Packaging the code'
                    echo "Packaging the code for ${params.APPVERSION} version"
                }
            }
        }
        stage('PublishToJfrog') {
            input {
                message "Do you want to publish the code to Jfrog artifactory?"
                ok "Yes,publish it"
            }
            steps {
                script {
                    echo 'Publishing the code to Jfrog artifactory'
                    sh 'mvn -U deploy -s settings.xml'
                }
            }
        }
    }
}
