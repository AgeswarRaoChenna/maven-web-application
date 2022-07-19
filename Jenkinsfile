node{
    
    def mavenHome = tool name: "maven3.8.6"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: '']])
    properties([[$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    stage('checkOutCode'){
        git branch: 'development', credentialsId: '57cfc1ff-9f3e-4f38-b2c7-6b70cab539ff', url: 'https://github.com/AgeswarRaoChenna/maven-web-application.git'
    }
    stage('build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('sonarQubeReport'){
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('uploadArtifactsToNexus'){
 	    sh "${mavenHome}/bin/mvn deploy"
    }
    stage('deployAppInToTomcatServer'){
        sshagent(['765e1d6b-37ff-4d3b-98c6-c88cb8cf7683']) {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.234.111.99:/opt/apache-tomcat-9.0.64/webapps"
        }
    }
}
