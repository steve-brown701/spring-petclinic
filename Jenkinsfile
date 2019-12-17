node ('ubuntu_node1') {
  def MVN = tool name: 'maven-3.6.3', type: 'maven'
  stage ('Checkout') {
    echo "Cloning git repo."
    git 'https://github.com/steve-brown701/spring-petclinic.git'
  }
  stage ('Build') {
    echo "Building project."
    sh "${MVN}/bin/mvn clean compile"
  }
  stage ('Test') {
    echo "Testing project"
    sh "${MVN}/bin/mvn test"
    junit '**/target/surefire-reports/TEST-*.xml'
  }
  stage ('Package') {
    echo "Packaging project"
    sh "${MVN}/bin/mvn package"
    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
  }
  stage ('Deploy') {
    echo "Deploying project"
    sh 'scp target/*.jar jenkins@192.168.56.12:/opt/petclinic/'
    // attempt to kill any running petclinic instance
    try {
      // create script on server
      sh 'ssh jenkins@192.168.56.12 "kill -9 $(ps -aef | grep spring-petclinic |awk '{print $2;exit}')"'
      //sh 'ssh jenkins@192.168.56.12 "chmod a+x kick.sh && ./kill.sh"'      
    } catch (Exception err) {
      echo "Error killing app!!!"
    } finally{
      // start application and exit
      sh 'ssh jenkins@192.168.56.12 \'nohup java -jar /opt/petclinic/spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar & 1>&2\''
      exit 0
    }
  }
}
