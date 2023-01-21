pipeline{
    agent{
        node{
            label "linux && java11"
        }
    }
    stages{
        stage("Build"){
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
            steps{
                echo("Hello Deploy 1")
                sleep(5)
                echo("Hello Deploy 2")
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
