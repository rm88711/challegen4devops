# challegen4devops

## Jenkins Docker

Criar um Dockerfile com a instrução:

```

FROM jenkins/jenkins:lts-jdk11
USER root
RUN apt update && \
    apt install -y --no-install-recommends gnupg curl ca-certificates apt-transport-https && \
    curl -sSfL https://apt.octopus.com/public.key | apt-key add - && \
    sh -c "echo deb https://apt.octopus.com/ stable main > /etc/apt/sources.list.d/octopus.com.list" && \
    apt update && apt install -y octopuscli
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

