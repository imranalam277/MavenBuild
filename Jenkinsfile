node {
    def sonarHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    
    stage('Code Checkout') {
        checkout changelog: false, poll: false, scm: scmGit(
            branches: [[name: '*/master']], 
            extensions: [], 
            userRemoteConfigs: [[credentialsId: 'GitHubCreds', url: 'https://github.com/anujdevopslearn/MavenBuild']]
        )
    }
    
    stage('Build Automation') {
        sh """
            ls -lart
            mvn clean install
            ls -lart target
        """
    }
    
    stage('Code Scan') {
        withSonarQubeEnv(credentialsId: 'SonarQubeCreds') {
            sh "${sonarHome}/bin/sonar-scanner"
        }
    }
    
    stage('Code Coverage Check') {
        script {
            def coverageJSON = sh(script: "curl -s 'http://35.154.151.174:9000/sonar/api/measures/component?componentKey=com.java.example:java-example&metricKeys=coverage'", returnStdout: true).trim()
            def sonarCoverage = readJSON text: coverageJSON
            def coverageValue = sonarCoverage.component.measures[0].value.toInteger()
            
            if (coverageValue >= 50) {
                echo 'Code coverage is above 50%. Passed!'
            } else {
                error 'Code coverage is below 50%. Failed!'
            }
        }
    }
    
    stage('Code Deployment') {
        deploy adapters: [tomcat9(credentialsId: 'TomcatCred', path: '', url: 'http://3.129.195.129:8080/')], contextPath: 'Planview', onFailure: false, war: 'target/*.war'
    }
}
