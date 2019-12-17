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
      sh 'ssh jenkins@192.168.56.12 "echo \"kill -9 $(ps -aef | grep spring-petclinic |awk '{print $2;exit}')\" >> kick.sh"'
      sh 'ssh jenkins@192.168.56.12 "./kick.sh"'      
    } catch (err) {
      echo err
    } finally{
      sh 'ssh jenkins@192.168.56.12 \'java -jar /opt/petclinic/spring-petclinic-2.2.0.BUILD-SNAPSHOT.jar &\''
    }
  }
}
