#!groovy

node('build-slave') {

   currentBuild.result = "SUCCESS"

   try {
      stage('Checkout'){
         if(!env.hub_org)
            error 'Please set a Jenkins environment variable named hub_org with value as registery/sunbidrded'
         checkout scm
         commit_hash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
         branch_name = sh(script: 'git name-rev --name-only HEAD | rev | cut -d "/" -f1| rev', returnStdout: true).trim()
         echo "Git Hash: "+commit_hash
         echo "branch_name: "+branch_name
       }

      stage('Build'){
        env.NODE_ENV = "build"
        print "Environment will be : ${env.NODE_ENV}"
        sh('git submodule update --init')
        sh('git submodule update --init --recursive --remote')
         sh('chmod 777 build.sh')
         sh("./build.sh ${commit_hash} ${branch_name} ${env.NODE_NAME} ${hub_org}")

      }
      stage('ArchiveArtifacts'){
           archiveArtifacts "metadata.json"
      }

    }
    catch (err) {
        currentBuild.result = "FAILURE"
        throw err
    }

}

