#!groovy
import groovy.json.JsonSlurperClassic
node 
{
    def toolbelt = tool 'sfdx_tool'
	
    /* stage('Clean Workspace')
	{
	
	 cleanWs()
	 
	 println 'WorkSpace Cleaned'
	 
	}*/
	
    stage('Checkout Branch') {
	 cleanWs()
	 println 'WorkSpace Cleaned'
	    
        checkout scm
        }
	
    stage('Selenium Test Scripts')
	{
	 
	 println 'TO DO'
         println 'java -cp "/opt/testng-6.8.jar:bin" org.testng.TestNG testng.xml'		
	 
	}
	
    stage('Approval') {
		input 'Do you approve deployment?'
               /* script {
                    def approvalNeeded = input id: 'Authenticate CI and Validate', message: 'Deploy to production?', submitter: 'chansk'
                }*/
           
	}
	
    stage('Authenticate CI and Validate')
         {   
	
        withCredentials([file(credentialsId: 'JWT_CRED_ID_DH', variable: 'JWT_Secret_CRT'), string(credentialsId: 'AUTO_SFDX_CONNECTED_APP_CONSUMER_KEY_DH', variable: 'cKey'), string(credentialsId: 'AUTO_SFDX_USERNAME', variable: 'uName'),string(credentialsId: 'SANDBOX_SFDC_HOST_DH', variable: 'hName')])
                {
                    echo "Client id ${cKey}"
					echo "UserName is ${uName}"
					echo "Instance Url is ${hName}"
			rc1 = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:logout --targetusername ${uName} -p"
					rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${cKey} --username ${uName} --jwtkeyfile \"${JWT_Secret_CRT}\" --instanceurl ${hName}"
                         if (rc != 0) 
						{ 
                          error 'org authorization failed' 
                        }
				    rc = bat returnStdout: true, script: "\"${toolbelt}\" force:source:deploy -p force-app/main/default -u ${uName} -c"
			                  
				}
    }
}
