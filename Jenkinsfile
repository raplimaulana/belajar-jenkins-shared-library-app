@Library("belajar-jenkins-shared-library@main")_

import belajar.jenkins.Output;

pipeline{
    agent any
    stages{
        stage("Hello Groovy"){
            steps{
                script{
                    Output.hello(this, "Groovy")
                }
            }
        }
        stage("Hello World"){
            steps{
                script{
                    hello.world()
                }
            }
        }
        stage("Global Variable"){
            steps{
                script{
                    echo(author())
                    echo(author.name())
                    echo(author.address())
                }
            }
        }
        stage("Maven Compile"){
            steps{
                script{
                    maven(["clean", "compile", "test"])
                }
            }
        }
        stage("Hello Person"){
            steps{
                script{
                  hello.person([
                    firstName: "Rapli",
                    lastName: "Maulana"
                  ])
                }
            }
        }
    }
}