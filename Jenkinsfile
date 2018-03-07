pipeline {
	agent { label 'Mac' }
  options{
  	timestamps()

  		 }

	parameters{
		string(defaultValue: 'fastlane/match', description: '', name: 'ARCH_BRANCH')
		string(defaultValue: 'develop', description: '', name: 'APPCORE_BRANCH')
		string(defaultValue: 'develop', description: '', name: 'UIKIT_BRANCH')
		string(defaultValue: 'GMA-5.x', description: '', name: 'SDK_BRANCH')
		string(defaultValue: 'develop', description: '', name: 'LOCALIZATION_BRANCH')
		}


  stages {

  	stage('SCM') {

		steps{
  		checkout([$class: 'GitSCM',
			  branches: [[name: 'develop']],
			  doGenerateSubmoduleConfigurations: false,
			  extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: true, recursiveSubmodules: true, reference: '', timeout: 15, trackingSubmodules: true], [$class: 'WipeWorkspace']],
			  submoduleCfg: [],
			  userRemoteConfigs: [[credentialsId: '3b905957-4471-4d89-a2a7-207b2dcc1630', url: 'ssh://git@coderepository.mcd.com:8443/archus/ios.git']]])
			}
  		}

        stage('Checkout') {
          steps {
		  sshagent(['3b905957-4471-4d89-a2a7-207b2dcc1630']) {
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
UIKIT_BRANCH=${GIT_BRANCH#*/}
SDK_BRANCH="GMA-5.x"


echo "*************Checking out ARCH_US*******************"
git checkout "develop"
git gc --prune=now
git clean -fdx
git pull


echo "*************Checking out Localization Repository*****************"

#cd "${LOCALIZATION_DIR}"
#git submodule
#git submodule init
#git submodule update --checkout --recursive
#git checkout feature/ARCH-32605


echo "*************Checking out McDAppCore Repository*****************"

cd "${MCDAPPCORE_DIR}"
git submodule
git submodule init
git submodule update --checkout --recursive
git checkout develop

echo "*************Checking out McDUIKit Repository*****************"

cd "${MCDUIKIT_DIR}"
git submodule
git submodule init
git submodule update --checkout --recursive
git checkout develop

echo "*************Checking out MCDONALDsSDK Repository*****************"

cd "${MCDCONNECT_DIR}"
git submodule
git submodule init
git submodule update --checkout --recursive
git checkout GMA-5.x
'''
          }
        }

	}
   stage('Build') {
      steps {
        sh '''
	/usr/libexec/PlistBuddy -c "Set :'Ensighten App ID' gma5-ios-datalayer-dev" Arch/Arch/Info.Plist
	
	  #run build
	 # unlock-keychain [-u] [-p password] [keychain]
	  security unlock-keychain -p "mactest" "/Users/mactest/Library/Keychains/login.keychain"
	#security unlock-keychain credentialsId: 'e8193c18-8c12-4317-ac97-a830dda83ec7' "/Users/mactest/Library/Keychains/login.keychain"

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

    stage('Hockeyapp') {
    	steps {
    			step([$class: 'HockeyappRecorder',
    			applications: [[apiToken: '6d02c4317eb5438eb67da9e06b752825',
    			downloadAllowed: false,
    			filePath: 'build/Arch.ipa',
    			mandatory: false,
    			notifyTeam: false,
    			releaseNotesMethod: [$class: 'NoReleaseNotes'],
    			uploadMethod: [$class: 'AppCreation', publicPage: false]]],
    			debugMode: false,
    			failGracefully: false])
    	 	 }

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

	
}
