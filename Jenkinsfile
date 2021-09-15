node {
    def registeryProject = "mugen-project/container_registry"
    def imageProject = "${registeryProject}:version-${env.BUILD_ID}"
    def img

    stage("Checkout") {
        checkout scm
    }

    // img : contient l'image qui sera creer au cours de cette etape
     
    def img = stage("Build") {
        docker.build("$imageProject" , ".")
    }

    stage("Run") {
       docker.image("$imageProject").withRun("--name image-$BUILD_ID -p 9090:90") {
            c -> 
                sh 'docker ps'
                // sh 'curl localhost:9090'
                echo 'run success'
        }
    }

    stage("Push") {
        docker.withRegistry('https://gitlab.com/Abdoulfatah12/mugen-project/container_registry') {  
            img.push 'latest' 
            img.push()
        }
    }


}

//step 1 : login as gitlab user => docker login registry.gitlab.com (username - password)
//step 2 : create image 
//step 3 : push une image sur gitlab container registery : docker push registry.gitlab.com/abdoulfatah12/mugen-project 

// stage : renvoie un pointeur null