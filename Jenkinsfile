def gv

pipeline {
    agent any
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }
    stages {
        stage("init") {
            steps {
                script {
                   gv = load "script.groovy" 
                }
            }
        }
        stage("build") {
            steps {
                script {
                    gv.buildApp()
                }
            }
        }
        stage("test") {
            when {
                expression {
                    params.executeTests
                }
            }
            steps {
                script {
                    gv.testApp()
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
        stage("copyfile") {
            steps {
                script {
                    swName="app"
                    currentDate=$(date "+%Y%m%d")
                    currentTime=$(date "+%Y%m%d-%H%M")
                    newFolderName=$swName-$currentTime
                    
                    go get -d -u -v || true
                                        
                    mkdir -p /home/idx/Desktop/buildtest/$newFolderName/$swName
                    cp -r /var/lib/jenkins/workspace/test01 /home/idx/Desktop/buildtest/$newFolderName/$swName
                    rm -rf /var/lib/jenkins/workspace/*
                }
            }
        }
    }   
}
