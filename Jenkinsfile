node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */
	git 'https://github.com/vasurampalli/NodeApp.git'

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image */
	//sh "sudo usermod -a -G docker $rampallidocker"
	//sh "sudo chmod 777 //var/run/docker.sock"

        app = docker.build("rampallidocker/nodeapp")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker-id') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
    }
    stage('Pull image') {
        /* 
			You would need to first register with DockerHub before you can pull images to your account
		*/
	steps {
        sh 'docker login -u rampallidocker -p Sriniwaas1234@'
        sh 'docker pull rampallidocker/nodeapp:latest    
        //docker.withRegistry('https://registry.hub.docker.com', 'docker-id') {
          //  app.pull("${env.BUILD_NUMBER}")
            //app.pull("latest")
            } 
                echo "Trying to Pull Docker Build to DockerHub"
    }
}
