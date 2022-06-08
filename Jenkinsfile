def mergeBeforeBuild() {

  // Pull the source and test a merge
  try {
    def sourceScmCheck = checkout changelog: true, poll: true, scm: [
      $class: 'GitSCM',
      branches: [[name: "origin/$sourceBranch" ]],
      doGenerateSubmoduleConfigurations: false,
      extensions: [[
        $class: 'PreBuildMerge',
        options: [
          fastForwardMode: 'FF',
          mergeRemote: 'origin',
          mergeStrategy: 'default',
          mergeTarget: "${env.target_branch}"]],
      ],
      submoduleCfg: [],
      userRemoteConfigs: [[
        credentialsId: 'gittoeken',
        name: 'origin',
        url: "$repoUrl"
      ]]
    ]
  } catch (error) {
    currentBuild.result = 'FAILURE'
    echo "ERROR: ${error}"
    sh 'false'
  }
}
node
  {
  stage('checkout')
    {
    checkout scm
    }
  stage('deploy')
    {
    echo 'branch name ' + env.BRANCH_NAME
    
    if (env.BRANCH_NAME.startsWith("Feature_"))
      {
      echo "Deploying to Dev environment after build"
      }
      
    else if (env.BRANCH_NAME.startsWith("Release_"))
      {
      echo "Deploying to Stage after build and Dev Deployment"
      }
      
    else if (env.BRANCH_NAME.startsWith("master"))
      {
      echo "Deploying to PROD environment"
      }
      
    sh """chmod +x HelloWorld.sh 
    ./HelloWorld.sh"""
    script{
      mergeBeforeBuild()
        
      }
 
    }
}
