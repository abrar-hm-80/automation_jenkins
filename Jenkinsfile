pipeline {
  agent any
  stages {
    stage('verify tooling') {
      steps {
        sh '''
          docker version
          docker info
          docker compose version 
          curl --version
          mvn --version
        '''
      }
    }

    stage('Maven build') {
      steps {
        sh './scripts/run_all.sh'
        sh 'docker system prune -a --volumes -f'
      }
    }

    stage('Start container') {
      steps {
        sh 'docker compose up -d --no-color --wait'
        sh 'docker compose ps'
      }
    }

    stage('Run tests against the container') {
      steps {
        //sh 'curl http://localhost:9090'
        echo "Test should be applied after the deployment on the different servers"
      }
    }

    stage('Deploiement en dev') {
      environment {
        KUBECONFIG = credentials('config')
      }
      steps {
        script {
          sh '''
rm -Rf .kube
mkdir .kube
ls
sudo touch .kube/config
sudo chmod 777 .kube/config
cat $KUBECONFIG > .kube/config
// cp fastapi/values.yaml values.yml
// cat values.yml
// sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
// helm upgrade --install app fastapi --values=values.yml --namespace dev
'''
        }

      }
    }

    stage('Deploiement en Test') {
      environment {
        KUBECONFIG = credentials('config')
      }
      steps {
        script {
          sh '''
rm -Rf .kube
mkdir .kube
ls
sudo touch .kube/config
sudo chmod 777 .kube/config
cat $KUBECONFIG > .kube/config
'''
        }

      }
    }

    stage('Deploiement en staging') {
      environment {
        KUBECONFIG = credentials('config')
      }
      steps {
        script {
          sh '''
rm -Rf .kube
mkdir .kube
ls
sudo touch .kube/config
sudo chmod 777 .kube/config
cat $KUBECONFIG > .kube/config
'''
        }

      }
    }

    stage('Deploiement en prod') {
      environment {
        KUBECONFIG = credentials('config')
      }
      steps {
        timeout(time: 15, unit: 'MINUTES') {
          input(message: 'Do you want to deploy in production ?', ok: 'Yes')
        }

        script {
          sh '''
rm -Rf .kube
mkdir .kube
ls
sudo touch .kube/config
sudo chmod 777 .kube/config
cat $KUBECONFIG > .kube/config
//cp fastapi/values.yaml values.yml
// cat values.yml
// sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
//helm upgrade --install app fastapi --values=values.yml --namespace prod
'''
        }

      }
    }

  }
}
