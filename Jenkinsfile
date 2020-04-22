pipeline{
  agent any 
  tools {
      maven 'apache-maven-latest'
      jdk 'adoptopenjdk-hotspot-jdk8-latest'
  }
  stages{
    stage("Maven Build"){
        steps {
            sh 'mvn -f lemminx-maven/pom.xml clean verify -B -Dmaven.test.error.ignore=true -Dmaven.test.failure.ignore=true -Dmaven.repo.local=$WORKSPACE/.m2/repository'
        }
        post {
			always {
				junit 'lemminx-maven/target/surefire-reports/TEST-*.xml'
				archiveArtifacts artifacts: 'lemminx-maven/target/*.jar'
			}
		}
    }
    stage ('Deploy Maven artifacts') {
      when {
          branch 'master'
      }
      steps {
        withMaven {
          sh 'mvn clean deploy -B -DskipTests -Dcbi.jarsigner.skip=false --file lemminx-maven/pom.xml'
        }
      }
    }
  }
}
