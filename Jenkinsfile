pipeline{
    options{
        timeout(time: 30, unit: 'MINUTES')\
        disableConcurrentBuilds()
        ansicolor('xterm')
    }
    parameters{
        choice(name: 'action', choices:['Apply', 'Destroy'], description: 'select something')
    }

stages{
    stage( 'Init'){
        steps {
            sh """
            cd 01-vpc
            terraform init -reconfigure
            """
        }
    }
     
     stage('Plan'){
        when{
            expression{
                params.action == 'apply'
            }
        }
        steps{
            
            sh """
            cd 01-vpc
            terraform plan 
            """

        }

     }
     stage  ('Apply'){
        steps{
            sh """
            cd 01-vpc
            terraform apply -auto-approve
            """
        }
     }

     stage ('Destroy') {
        when{
            expression{
                params.action== 'destory'
            }
        }
        steps{
            sh """
            cd 01-vpc
            terraform destory -auto-approve
            """
        }
     }
}
post{
    always{
        echo 'Job is success or failure this block will be running'
        deleteDir()
    }
    success{
        echo 'This block will be running only at success of build'
    }
    failure{
        echo 'This block will be running only at failure of build'
    }
}
}