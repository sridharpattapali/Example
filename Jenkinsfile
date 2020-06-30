#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    def toolbelt = tool 'toolbelt'

    stage('Checkout Branch') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Validate code in CI') {
            rc = command "${toolbelt}/sfdx force:auth:jwt:grant --clientid 3MVG97quAmFZJfVylQ42coJwBXR.ari2CiMqbtW02dWJcHpF75o3yST8bTOvDtzWMmDMmycdHx7VWibJh.hxm --username chand@ci.com --jwtkeyfile C:\Users\chansk\Documents\MySalesforceProjects\GP\openssl\server.key --instanceurl https://login.salesforce.com"
            if (rc != 0) {
			println 'rc is:'
			println rc
             error 'Salesforce dev hub org authorization failed.'
            }
        }
    }
}