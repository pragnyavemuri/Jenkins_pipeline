pipeline {
  agent any
  options{
  	timestamps()

  		 }

	parameters{
		string(defaultValue: 'master', description: '', name: 'ARCH_BRANCH')
		string(defaultValue: 'master', description: '', name: 'APPCORE_BRANCH')
		string(defaultValue: 'master', description: '', name: 'UIKIT_BRANCH')
		string(defaultValue: 'master', description: '', name: 'SDK_BRANCH')}


  stages {

  	stage('SCM') {

		steps{
  		checkout([$class: 'GitSCM',
			  branches: [[name: 'De-develop']],
			  doGenerateSubmoduleConfigurations: false,
			  extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: true, recursiveSubmodules: true, reference: '', timeout: 15, trackingSubmodules: true]],
			  submoduleCfg: [],
			  userRemoteConfigs: [[credentialsId: 'ef683bd0-363a-4e07-878d-33510fe96fe8', url: 'https://git-codecommit.us-east-2.amazonaws.com/v1/repos/mcdonalds-gma5x-android']]])
			}
  		}

        stage('Checkout') {
          steps {
            sh '''
			MCDARCH_DIR=${WORKSPACE}
			MCDAPPCORE_DIR="${MCDARCH_DIR}/libraries/android-mcd-core-app"
			MCDUIKIT_DIR="${MCDARCH_DIR}/libraries/android-mcd-core-app/libraries/android-mcd-uikit"
			MCDCONNECT_DIR="${MCDARCH_DIR}/libraries/android-mcd-core-app/libraries/android-gma-sdk-sapient"

			cd "${MCDARCH_DIR}"
			git checkout ${ARCH_BRANCH}
			git gc --prune=now
			git clean -fdx
			git pull

			cd "${MCDAPPCORE_DIR}"
			git checkout ${APPCORE_BRANCH}
			git gc --prune=now
			git clean -fdx
			git pull

			cd "${MCDUIKIT_DIR}"
			git checkout ${UIKIT_BRANCH}
			git gc --prune=now
			git clean -fdx
			git pull

			cd "${MCDCONNECT_DIR}"
			git checkout ${SDK_BRANCH}
			git gc --prune=now
			git clean -fdx
			git pull'''
          }
        }


  /* stage('Build') {
      steps {
        sh '''
	  #run build
	 # ./gradlew clean assembleQARelease
	 '''
    }
    } */

/*stage('Test') {
      steps {
        sh './gradlew cleanTest test jacocoTestReport --continue --info'
      }
    }
*/

 /*   stage('Hockeyapp') {
    	steps {
    			step([$class: 'HockeyappRecorder',
    			applications: [[apiToken: 'd90ccdd12e7440518048400adee5bb23',
    			downloadAllowed: false,
    			filePath: 'app/build/outputs/apk/app-QA-release.apk',
    			mandatory: false,
    			notifyTeam: false,
    			releaseNotesMethod: [$class: 'NoReleaseNotes'],
    			uploadMethod: [$class: 'AppCreation', publicPage: false]]],
    			debugMode: false,
    			failGracefully: false])
    	 	 }
}*/
    } 
 /*	post {

		success {

			emailext (
          		subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
          		body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
          		  <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
         		recipientProviders: [[$class: 'rushikesh.jawali@capgemini.com']]
				)

		}


	        failure {

			emailext (
          		subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
          		body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
          		  <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
          		recipientProviders: [[$class: 'rushikesh.jawali@capgemini.com']]
       			 	)
            
        }
    } */
}
