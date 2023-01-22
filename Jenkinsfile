pipeline{
    agent none
    environment{
        AUTHOR = "Rapli Maulana Aji"
        EMAIL = "rapli.maulana@gmail.com"
    }
    parameters{
        string(name: "NAME", defaultValue: "Guest", description: "What is your name?")
        text(name: "DESCRIPTION", defaultValue: "", description: "Tell me about you?")
        booleanParam(name: "DEPLOY", defaultValue: false, description: "Need to deploy?")
        choice(name: "SOCIAL_MEDIA", choices: ["Instagram", "Facebook", "Twitter"], description: "Which social media you used?")
        password(name: "SECRET", defaultValue: "", description: "Encrypt Key")
    }
    options{
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
    }
    stages{
        stage("OS Setup"){
            matrix{
                axes{
                    axis{
                        name "OS"
                        values "linux", "windows", "mac"
                    }
                    axis{
                        name "ARC"
                        values "32", "64"
                    }
                }
                excludes{
                    exclude{
                        axis{
                            name "OS"
                            values "mac"
                        }
                        axis{
                            name "ARC"
                            values "32"                   
                        }
                    }
                }
                stages{
                    stage("OS Setup"){
                        agent{
                            node{
                                label "linux && java11"
                            }
                        }
                        steps{
                            echo("Setup ${OS} ${ARC}")
                        }
                    }    
                }
            }
        }
        stage("Preparation"){
            failFast true
            parallel{
                stage("Prepare Java"){
                    agent{
                        node{
                            label "linux && java11"
                        }
                    }
                    steps{
                        echo "Prepare Java"
                        sleep(5)
                    }
                }
                stage("Prepare Maven"){
                    agent{
                        node{
                            label "linux && java11"
                        }
                    }
                    steps{
                        echo "Prepare Maven"
                        sleep(5)
                    }
                }
            }
        }
        stage("Parameter"){
            agent{
                node{
                    label "linux && java11"
                }
            }
            steps{
                echo("Hello : ${params.NAME}")
                echo("Description: ${params.DESCRIPTION}")
                echo("Deploy: ${params.DEPLOY}")
                echo("Social Media: ${params.SOCIAL_MEDIA}")
                echo('Secret: $params.SECRET')
            }
        }
        stage("Prepare"){
            environment{
                APP = credentials("maulana_rahasia")                 //mengambil data pada credential dengan ID 'maulana_rahasia'. Secara otomatis username akan menjadi APP_USR dan password menjadi APP_PSW
            }
            agent{
                node{
                    label "linux && java11"
                }
            }
            steps{
                echo("Author : ${AUTHOR}")
                echo("Email : ${EMAIL}")
                echo("Start Job : ${env.JOB_NAME}")
                echo("Start Build : ${env.BUILD_NUMBER}")
                echo("Branch Name : ${env.BRANCH_NAME}")
                echo("App User : ${APP_USR}")
                echo("App Password : ${APP_PSW}")
                sh('echo "App Password : $APP_PSW" > "rahasia.txt"')
            }
        }
        stage("Build"){
            agent{
                node{
                    label "linux && java11"
                }
            }
            steps{
                script{
                    for (int i=0; i<10; i++){
                        echo("Script ${i}")
                    }
                }
                echo("Start Build")
                sh("./mvnw clean compile test-compile")
                echo("Finish Build")
            }
        }
        stage("Test"){
            agent{
                node{
                    label "linux && java11"
                }
            }
            steps{
                script{
                    def data = [
                        "FirstName" : "Rapli",
                        "LastName"  : "Maulana"
                    ]
                    writeJSON(file: "data.json", json: data)
                }
                echo("Start Build")
                echo("./mvnw test")
                echo("Finish Build")
                }
            }
        stage("Deploy"){
            input{
                message "Can we deploy?"
                ok "Yes, of course"
                submitter "rapli,maulana,aji"
                parameters{
                    choice(name: "TARGET_ENV", choices: ["DEV","QA","PROD"], description: "We will deploy to?")
                }
            }
            agent{
                node{
                    label "linux && java11"
                }
            }
            steps{
                echo("Hello Deploy 1")
                sleep(5)
                echo("Hello Deploy 2")
                echo("Deploy to ${TARGET_ENV}")
            }
        }
        stage("Release"){
            when{
                expression{
                    return params.DEPLOY         
                }
            }
            agent{
                node{
                    label "linux && java11"
                }
            }
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "maulana_rahasia",
                    usernameVariable: "USER",                                  
                    passwordVariable: "PASSWORD"                           
                )]){
                    sh('echo "Release with -u $USER -p $PASSWORD"')
                }
            }
        }
    }
    post{
        always{
            echo "I will always say Hello again"
        }
        success{
            echo "Yeay, success"
        }
        failure{
            echo "Oh no, failure"
        }
        cleanup{
            echo "Don't care success or error"
        }
    }
}
