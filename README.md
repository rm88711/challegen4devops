# Challegen4devops


## Pipeline para aplicar a api na Azure

```
node {

  def resourceGroupName = 'rg-challege4-pr-api'
  def resourceGroupLocation = 'brazilsouth'
  def appServicePlanName = 'prikkasPlan'
  def appServicePlanTier = 'FREE'
  def webAppName = 'prikka-api'
  def webAppRuntime = '"java:11:Java SE:11"'
  def packagePath = 'target/prikkas-0.0.1-SNAPSHOT.jar'

  stage('Extrair Codigo Fonte') {
    echo 'Obtendo o Código Fonte ...'
    checkout([$class: 'GitSCM', branches: [[name: '*/branch-api']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/rm88711/prikkasApi']]])
  }

  stage('Build') {
    echo 'Empacotando o projeto...'
    sh 'mvn clean package'
  }

  stage('Credenciais Azure') {
    echo 'Obtendo credenciais...'
    withCredentials([usernamePassword(credentialsId: 'AzureService', 
      passwordVariable: 'AZURE_CLIENT_SECRET',
      usernameVariable: 'AZURE_CLIENT_ID')]) {
      echo 'Logando na Azure...'
      sh 'az login -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET'
    }
  }

  stage('Criar Infra') {
    echo 'Criando o Grupo de Recursos...'
    sh "az group create --name $resourceGroupName --location $resourceGroupLocation"
    echo 'Criando Plano de Serviço...'
    sh "az appservice plan create --name prikkasPlan --resource-group rg-challege4-pr-api --sku FREE"
    echo 'Criando o Web App...'
    sh "az webapp create --name $webAppName --plan $appServicePlanName --resource-group $resourceGroupName --runtime $webAppRuntime"
  }

  stage('Deploy') {
     echo 'Realizando o Deploy na Azure...'
     sh "az webapp deploy --resource-group $resourceGroupName --name $webAppName --src-path $packagePath --type jar"
  }

}

```



## Jenkins Docker

referencia:
https://octopus.com/blog/jenkins-docker-install-guide

Criar um Dockerfile com a instrução:

```

FROM jenkins/jenkins:lts-jdk11
USER root
RUN apt update && \
    apt install -y --no-install-recommends gnupg curl ca-certificates apt-transport-https && \
    curl -sSfL https://apt.octopus.com/public.key | apt-key add - && \
    sh -c "echo deb https://apt.octopus.com/ stable main > /etc/apt/sources.list.d/octopus.com.list" && \
    apt update && apt install -y octopuscli && \
    apt -y install maven
USER jenkins

```

Abrir o docker desktop.

Abrir o Power Shell entrar na pasta onde esta o Dockerfile.

Exemplo :

```

C:\Users\kleber.monteiro\projetos\fiap\jenkins\

```

Na pasta executar o comando abaixo para criar uma imagem no docker.

```

docker build . -t jenkins

```

Executar o docker run para criar um container, lembre-se de criar um volume com o endereço do jenkins home para poder acessa a key depois: 


```

docker run --name jenkins -it -d -p 8080:8080 -p 50000:50000 -v /c/users/kleber.monteiro/projetos/fiap/jenkins/jenkins_home:/var/jenkins_home jenkins

```

Depois disso o Jenkins demora entre 5 e 10 minutos para iniciar totalmente.

Endereço do Jenkins Local:

```

http://localhost:8080

```
