node{

    def server = Artifactory.server 'artifactory'

    def rtGradle = Artifactory.newGradleBuild()
    
    withCredentials([usernamePassword(
        credentialsId: 'artifactory',
        usernameVariable: 'USERNAME', 
        passwordVariable: 'PASSWORD'
    )]) {
        server.username = "${USERNAME}"
        server.password = "${PASSWORD}"
    }

    def builfInfo

    stage('clone'){
        git url: 'https://github.com/cloudacademy/devops-webapp.git'
    }

    stage('Artifactory Config'){
        rtGradle.tool = "gradle-4.10.2"
        rtGradle.deployer repo:'gradle-release-local', server: server
        rtGradle.reolver repo:'jcenter', server: server

    }

    stage('build'){

        rtGradle.run rootDir: "./", buildFile: 'build.gradle', tasks: 'clean build'

    }

    stage("publish"){
        buildInfo = rtGradle.run rootDir: "./", buildFile: 'build.gradle', tasks: 'artifactoryPublish'
    }

    stage("deploy"){
        server.publishBuildInfo buildInfo
    }
}
