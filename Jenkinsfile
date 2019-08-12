node {
 	// Clean workspace before doing anything
    deleteDir()

    try {
        stage ('Clone') {
        	checkout scm
        }
        stage ('Build') {
		def response = serviceNow_createChange serviceNowConfiguration: [instance: 'exampledev', producerId: 'ls98y3khifs8oih3kjshihksjd'], credentialsId: 'jenkins-vault', vaultConfiguration: [url: 'https://vault.example.com:8200', path: 'secret/for/service_now/']
def jsonSlurper = new JsonSlurper()
def createResponse = jsonSlurper.parseText(response.content)
def sysId = createResponse.result.sys_id
def changeNumber = createResponse.result.number
        	sh "echo 'shell scripts to build project...'"
        }
        stage ('Tests') {
	        parallel 'static': {
	            sh "echo 'shell scripts to run static tests...'"
	        },
	        'unit': {
	            sh "echo 'shell scripts to run unit tests...'"
	        },
	        'integration': {
	            sh "echo 'shell scripts to run integration tests...'"
	        }
        }
      	stage ('Deploy') {
            sh "echo 'shell scripts to deploy to server...'"
      	}
    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
}




