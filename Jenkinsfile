#!/usr/bin/groovy
//def artifactUrl = "http://10.1.2.1:8081/nexus/content/repositories/snapshots/openshift/getting-started-tomcat/1.0-SNAPSHOT/getting-started-tomcat-1.0-20161026.133132-1.war"

node('fabric8') {
  def imageStreamName = 'os-sample-java-web'
  def buildNamespace = 'build'
  def devReleaseTag = 'dev-release'
  def bcName = 'os-sample-java-web'

  stage 'Checkout'
  checkout scm
  //git url: 'https://github.com/monodot/os-sample-java-web.git'
  echo 'Checkout complete'

  stage 'Maven Build'
  sh 'mvn clean deploy'
  echo 'Maven build complete'

  stage 'OpenShift Build'
  def pom = readMavenPom file: 'pom.xml'
  def artifactUrl = "http://nexus.ci.svc.cluster.local:8081/nexus/service/local/artifact/maven/content?r=snapshots&g=${pom.groupId}&a=${pom.artifactId}&v=${pom.version}&p=${pom.packaging}"
  openshiftBuild buildConfig: bcName, namespace: buildNamespace, verbose: 'true', showBuildLogs: 'true', env: [ [ name : 'WAR_FILE_URL', value : artifactUrl ] ]
  openshiftTag(namespace: buildNamespace, sourceStream: imageStreamName, sourceTag: 'latest', destinationStream: imageStreamName, destinationTag: "${pom.version}")
  openshiftTag(namespace: buildNamespace, sourceStream: imageStreamName, sourceTag: 'latest', destinationStream: imageStreamName, destinationTag: devReleaseTag)
  echo 'OpenShift build complete'

}
