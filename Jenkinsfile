node {
// Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    def server = Artifactory.server "jfrog-server"
    def buildInfo = Artifactory.newBuildInfo() 

// Project Dir
    def project_path = "01-Jenkins/petclinic-code"


stage('GitCheckOut') {
git branch: 'main',  url: 'https://github.com/amitvashisttech/devops301-mindtree-09-Jan-2021.git'
}



dir(project_path){

stage('Maven Clean') {
sh 'mvn clean'
}

stage('Maven Compile') {
sh 'mvn compile'
}

stage('Maven test') {
sh 'mvn test'
}

stage('Sonar-Code-Analysis'){
sh 'mvn sonar:sonar -Dsonar.host.url=http://rlzvt92632dns.eastus2.cloudapp.azure.com:9000' 
}
stage('Maven pkg') {
sh 'mvn package'
}

stage('Build Managment') {
      def uploadSpec = """{
	 "files": [
	{
	"pattern": "**/*.war",
	"target": "devops_301_/petclinic.war"
        }
	]
	}"""
	server.upload spec: uploadSpec
 }


stage('Archive Artifacts') {
archive 'target/*.war'    
}
}

stage('Tomcat Deployment'){
 deploy adapters: [tomcat8(credentialsId: 'tomcat01', path: '', url: 'http://http://fomop41962dns.eastus2.cloudapp.azure.com:8080/')], contextPath: 'petclinic', war: 'files/petclinic.war'
}
 
 
}
