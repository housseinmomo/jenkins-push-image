node {
    def registeryProject = "registry.gitlab.com/abdoulfatah12/mugen-project"
    def imageProject = "${registeryProject}:version-${env.BUILD_ID}"
    def img

    stage("Checkout") {
        checkout scm
    }  
    
    // img : contient l'image qui sera creer au cours de cette etape 
   stage("Build") {
       //  sh 'sudo service start docker'
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

    stage("Push-Gitlab") {
        

       sh 'docker login registry.gitlab.com -u Abdoulfatah12 -p Malyounhouss123' 
        
       sh "docker push $imageProject"
        
       echo "Push success"
            
    }
    stage("Create Backup")  {
     
      sh 'docker login -u abdoulfatah123 -p malyoun123'
        
      sh "docker tag $imageProject abdoulfatah123/drfanel:backup-$BUILD_ID"  
        
      sh "docker push abdoulfatah123/drfanel:backup-$BUILD_ID"
    }
    
    stage("Remove-image") {
        
        sh "docker image rm -f $imageProject abdoulfatah123/drfanel:backup-$BUILD_ID"
        
        sh 'docker images'
        
        echo "image remove"
    } 

}

//step 1 : login as gitlab user => docker login registry.gitlab.com (username - password)
//step 2 : create image 
//step 3 : push une image sur gitlab container registery : docker push registry.gitlab.com/abdoulfatah12/mugen-project 

// stage : renvoie un pointeur null
