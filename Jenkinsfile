node('agent'){
  def apiURL = 'https://192.168.2.200:8443'
  def namespace = 'default'

  stage 'Build'
  def buildConfig = 'ruby-sample-build'
  step new com.openshift.jenkins.plugins.pipeline.OpenShiftBuilder(apiURL, buildConfig, namespace, '', 'false', '', '', 'false', '')
  echo 'built'

  stage 'Deploy'
  def deploymentConfig = 'frontend'
  step new com.openshift.jenkins.plugins.pipeline.OpenShiftDeployer(apiURL, deploymentConfig, namespace, '', '')
  echo 'deployed'
}

