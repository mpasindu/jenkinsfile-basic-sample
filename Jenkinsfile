import groovy.json.JsonSlurper
node {


 	// Clean workspace before doing anything
    deleteDir()

    try {
        stage ('Clone') {
        	checkout scm
        }
        stage ('Build') {
        	sh "echo 'shell scripts to build project...'"
        }
	stage ('Create Change'){
		def response = serviceNow_createChange serviceNowConfiguration: [instance: 'clearmedev', producerId: 'de043421db74e340aa76bb423996198b'], credentialsId: 'snow'
		def jsonSlurper = new JsonSlurper()
		def createResponse = jsonSlurper.parseText(response.content)
		//println(createResponse)
		def sysId = createResponse.result.sys_id
		//println(sysId)
		def changeNumber = createResponse.result.number
		//println(changeNumber)
		echo changeNumber
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
	    sh "python /var/lib/jenkins/scripts/approval.py"
      	}
    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
}




