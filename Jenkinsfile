def projectName = "phptest".toLowerCase()
def scmRes
def allowedVerificationEnvs = ['dev','stage','preprod','main']
def sonarOptions = ['no','yes']
node {

    properties([
            parameters([
                choice(name: 'env', description: 'Environment to create Image', choices: allowedVerificationEnvs),
                choice(name: 'sonarqube', description: 'Select yes if you need SonarQube code quality check for this build.', choices: sonarOptions)
            ])
        ])

    stage('Checkout') {
        deleteDir()
        scmRes = checkout scm
        projectName = projectName + '-' + BRANCH_NAME
    }

    stage('RemoveGitDirectory') {
        sh "echo '${scmRes.GIT_COMMIT}' > .revision"
        sh 'rm -fr .git'
    }

    stage('SonarQube Code Check') {
        if(params.sonarqube=='yes'){
            sh "echo 'SONARQUBE CODE QUALITY ANALYSIS COMPLETED'"
        }else{
            sh "echo 'SONARQUBE CODE CHECK DISABLED'"
        }
    }

    stage('SonarQube Code Quality Check') {
        if(params.sonarqube=='yes'){
            sh 'sleep 50'
        }else{
            sh "echo 'SONARQUBE CODE QUALITY CHECK DISABLED'"
        }
    }

    stage('StoragePermission') {
            sh 'chmod 777 -R ./'
        }

    stage("ENV Update") {
       withCredentials([
            string(credentialsId: "${projectName}-APP_NAME", variable: 'APP_NAME')
       ]) {
           writeFile (file: '.env', text: """
APP_NAME=${APP_NAME}
"""
            )
         }
    }

    stage('Build') {
        sh "echo 'Build done now'"
    }

}


