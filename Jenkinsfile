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
                    echo(author.name())
                    echo(author.channel())
                }
            }
        }
    }
}