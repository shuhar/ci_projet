pipeline {
agent any
environment {
NEXUS_VERSION = "nexus3"
NEXUS_PROTOCOL = "http"
NEXUS_URL = "localhost:8081/"
NEXUS_REPOSITORY = "TestRepo"
NEXUS_CREDENTIAL_ID = "nexus-credentials"
}
stages {

	
stage('Build') {
steps {
echo 'Building..'
bat label: '', script: '''cd ./maven-demo-1
mvn clean install'''
echo 'Building.Done.'
}
}
stage('Test') {
steps {
echo 'Testing..'
bat label: '', script: '''cd ./maven-demo-1
mvn clean test'''
echo 'Testing. Done.'
}
}
stage('Deploy') {
steps {
echo 'Deploying....'
}
}
stage('publish to nexus') {
steps {
echo 'publish to nexus..'
script {
// Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
pom = readMavenPom file: "./maven-demo-1/pom.xml";
// Find built artifact under target folder
filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
// Print some info from the artifact found
echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
// Extract the path from the File found
artifactPath = filesByGlob[0].path;
// Assign to a boolean response verifying If the artifact name exists
artifactExists = fileExists artifactPath;
if(artifactExists) {
echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
nexusArtifactUploader(
nexusVersion: NEXUS_VERSION,
protocol: NEXUS_PROTOCOL,
nexusUrl: NEXUS_URL,
groupId: pom.groupId,
version: pom.version,
repository: NEXUS_REPOSITORY,
credentialsId: NEXUS_CREDENTIAL_ID,
artifacts: [
// Artifact generated such as .jar, .ear and .war files.
[artifactId: pom.artifactId,
classifier: '',
file: artifactPath,
type: pom.packaging],
// Lets upload the pom.xml file for additional information for Transitive dependencies
[artifactId: pom.artifactId,
classifier: '',
file: "pom.xml",
type: "pom"]
]
);
} else {
error "*** File: ${artifactPath}, could not be found";
}
}
}
}

}
post {
always {
emailext body: 'Bonjour', subject: 'test jenkis', to: 'mohamedamine.sadfi@esprit.tn'
}

}
}
