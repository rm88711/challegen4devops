# Challegen4devops

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
    apt -y install maven && \
    apt -y install azure-cli
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
