pipeline{
  agent none
  stages{
    stage('Compile'){
      agent any
      steps{
        sh 'mvn compile'
      }       
    }
    stage('Package'){
      agent any
      steps{
        sh 'mvn package'
      }       
    }
    stage('Test'){
      agent any
      steps{
        sh 'mvn test'
      }       
    }
    stage('Artifact'){
      agent any
      steps{
        sh 'upload war file to Artifactory'
      }       
    }
    stage('Deploy'){
      agent any
      steps{
        sh '''rm -rf dockerimages
mkdir dockerimages
cd dockerimages
cp /var/lib/jenkins/workspace/pipelineproject/gameoflife-web/target/gameoflife.war .
touch Dockerfile
cat<<EOT>> Dockerfile
FROM tomcat:8.0.15-jre7
ADD gameoflife.war /usr/local/tomcat/webapps
CMD ["catalina.sh" , "run"]
EXPOSE 8080
EOT
#sudo docker build -t tomcatserver .
sudo docker build -t tomcatserve:$BUILD_NUMBER .
#sudo docker run -itd --name tomcat -p 8888:8080 tomcatserver
sudo docker run -itd --name tomcat$BUILD_NUMBER tomcatserve:$BUILD_NUMBER'''
      }       
    }
  }
}
