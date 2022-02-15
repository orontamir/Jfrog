pipeline {
  environment {
    registry = "hagitw/spring-petclinic"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  tools {
    maven 'Maven 3.6.3'
    jdk 'jdk11'
  } 
  stages {
    stage('Cloning Git') {
      steps {
        git clone https://github.com/spring-projects/spring-petclinic.git
      }
    }
    stage('Compile') {
       steps {
         ./mvnw package
       }
    }
    stage('Run') {
      steps {
       java -jar target/*.jar
      }
    }
    stage('Building Image') {
      steps{
        script {
          dockerImage = docker.build registry + ":latest"
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
	stage('Unit Test') {    
      	steps{
	      		dir("spring-petclinic/src/test/jmeter/"){
	        		sh 'petclinic_test_plan.jmx'
	    	 	}
      		
      	}
    
  }
}
