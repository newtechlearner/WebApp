
node {
    // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    //def server = Artifactory.server "artifactory"
    // Create an Artifactory Maven instance.
    //def rtMaven = Artifactory.newMavenBuild()
    //def buildInfo
    
    //rtMaven.tool = "maven"
    
    
    
      stage('Clone sources') {
        git url: 'https://github.com/newtechlearner/webapp.git'
      }
    
      stage('Maven Build'){
        def mvnHome = tool name: 'maven', type: 'maven'
		sh "${mvnHome}/bin/mvn clean package -Dmaven.test.skip=true"
      }
		
	  stage('Deploy QA'){
    	deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://3.17.208.192:8080/')], contextPath: 'QAWebapp', war: '**/*.war'
      }	
    
      stage('BUILD') {
	  def mvnHome = tool name: 'maven', type: 'maven'
	  dir ('functionaltest') {
                sh "${mvnHome}/bin/mvn test"
          }
              
	/*Below works well*/	   
        /*  def mvn_version = 'maven'
          withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
              dir ('functionaltest') {
                //sh "cd functionaltest"
                sh "mvn test"      
              }
            
          }*/
          publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\functionaltest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
      }

    /*stage('Artifactory configuration') {
        // Tool name from Jenkins configuration
        rtMaven.tool = "maven"
        // Set Artifactory repositories for dependencies resolution and artifacts deployment.
        rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
    }

    stage('Maven build') {
        buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install'
    }

    stage('Publish build info') {
        server.publishBuildInfo buildInfo
    }*/
}
     
