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
			  branches: [[name: 'AU_ORDERING/DEV_RC2']],
			  doGenerateSubmoduleConfigurations: false,
			  extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: true, recursiveSubmodules: true, reference: '', timeout: 15, trackingSubmodules: true]],
			  submoduleCfg: [],
			  userRemoteConfigs: [[credentialsId: 'fcb0912a-1fc2-4f17-9951-bc6b60b74845', url: 'http://HygieiaUser@coderepository.mcd.com/scm/gma5_aus/ios.git']]])
			}
  		}

        stage('Checkout') {
          steps {
            sh '''
			echo "*******************Updating submodule*************************"
MCDARCH_DIR=${WORKSPACE}
LOCALIZATION_DIR="${MCDARCH_DIR}/Arch/Localization"
MCDAPPCORE_DIR="${MCDARCH_DIR}/AppCore"
MCDUIKIT_DIR="${MCDARCH_DIR}/AppCore/McDAppCore/Frameworks/submodule_McDUIKit"
MCDCONNECT_DIR="${MCDARCH_DIR}/AppCore/McDAppCore/Frameworks/submodule_McDonaldsSDK"
ARCH_BRANCH=${GIT_BRANCH#*/}
LOCALIZATION_BRANCH=${GIT_BRANCH#*/}
APPCORE_BRANCH=${GIT_BRANCH#*/}
UIKIT_BRANCH="AU-DEVELOP"
SDK_BRANCH="AU-DEVELOP"


echo "*************Checking out ARCH_US*******************"
git checkout "${ARCH_BRANCH}"
git gc --prune=now
git clean -fdx
git pull


echo "*************Checking out Localization Repository*****************"

cd "${LOCALIZATION_DIR}"
git submodule
git submodule init
git submodule update --checkout --recursive
git checkout "${LOCALIZATION_BRANCH}"


echo "*************Checking out McDAppCore Repository*****************"

cd "${MCDAPPCORE_DIR}"
git submodule
git submodule init
git submodule update --checkout --recursive
git checkout "${APPCORE_BRANCH}"

echo "*************Checking out McDUIKit Repository*****************"

cd "${MCDUIKIT_DIR}"
git submodule
git submodule init
git submodule update --checkout --recursive
git checkout "${UIKIT_BRANCH}"

echo "*************Checking out MCDONALDsSDK Repository*****************"

cd "${MCDCONNECT_DIR}"
git submodule
git submodule init
git submodule update --checkout --recursive
git checkout "${SDK_BRANCH}"
'''
          }
        }


   stage('Build') {
      steps {
        sh '''
	  #run build
	security unlock-keychain credentialsId: 'e8193c18-8c12-4317-ac97-a830dda83ec7' "/Users/mactest/Library/Keychains/login.keychain"

        # Install any missing required gems
        bundle install

        # Fastlane
        bundle exec fastlane qa_mcd configuration:QA
	 '''
    }
    } 

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
