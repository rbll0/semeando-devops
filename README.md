# Projeto de Deploy com Docker e Azure - Semeando 🌱




## Descrição do Projeto

O Semeando 🌱 é um aplicativo focado em educar adolescentes e jovens-adultos sobre sustentabilidade e energia renovável. Para o desenvolvimento, utilizamos o Docker para criar os contêineres necessários, garantindo que todos os serviços rodem de forma isolada e eficiente. O projeto inclui uma aplicação backend em Java, um serviço complementar em .NET e um banco de dados Oracle. Tudo foi orquestrado utilizando o Docker Compose, permitindo que os serviços funcionem juntos de maneira integrada e facilitando o deploy e o gerenciamento do ambiente.

## Requisitos

- Docker Instalado e configurando na Virtual Machine da Azure.
- Git para clonar o repositório.
- Conta na Microsoft Azure para criação da VM.

## Deployment
### 1. Criação da Virtual Machine na Azure
        - Sistema Operacional: Ubuntu Server 20.04 LTS.
        - Tamanho da máquina: Standard B1ms (baixo custo).

#### Intalação e configuração

Depois de instalar o Docker, rode o seguinte comando:
```sh
   docker -- version
```

#### Instrução na VM
Para o deployment correto será necessário o arquivo Dockerfile que são configurados nas aplicações Java e .Net, onde é configurado as dependências e o Banco de dados, outro arquivo necessário será o docker-compose.yml que é onde definimos as variáveis de ambiente e portas para acesso externo além da execução dos serviços.

Para realizar o deploy dos contêineres:

**Clone este repositório na VM (JAVA):**

    https://github.com/rbll0/semeando-spring.git
    cd semando-spring

**Clone este repositório na VM (.NET):**

    https://github.com/rbll0/semeando-spring.git
    cd semando-spring(TROCAR PARA O CERTO)


Execute o Docker Compose para construir e iniciar os serviços:

    docker-compose up --build -d

Verifique se os contêineres estão em execução:

    docker-compose ps

### Verificação

Para testar a aplicação, use as portas definidas no docker-compose.yml:

**Aplicação Java:** Porta 8080
**Aplicação .NET:** Porta 5000
**Banco de Dados Oracle:** Porta 1521

### Dockerfiles e docker-compose

Dockerfile Java
```sh
  # Etapa base: Ambiente de build
  FROM maven:3.9.4-eclipse-temurin-17 AS base
  
  # Define o diretório de trabalho
  WORKDIR /app
  
  # Copia o código-fonte para o container
  COPY . .
  
  # Compila o projeto sem rodar os testes
  RUN mvn clean install -DskipTests
  
  # Etapa final: Imagem otimizada com JRE
  FROM eclipse-temurin:17-jre
  
  # Define o diretório de trabalho
  WORKDIR /app
  
  # Expõe a porta 8080
  EXPOSE 8080
  
  # Copia o arquivo JAR gerado na etapa anterior
  COPY --from=base /app/target/spring-semeando-0.0.1-SNAPSHOT.jar app.jar
  
  # Define o comando de entrada para executar a aplicação
  ENTRYPOINT ["java", "-jar", "app.jar"]
```

Dockerfile .NET

```sh
    FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build-env
    WORKDIR /App
    
    # Copy everything
    COPY . ./
    # Restore as distinct layers
    RUN dotnet restore
    # Build and publish a release
    RUN dotnet publish -c Release -o out
    
    # Build runtime image
    FROM mcr.microsoft.com/dotnet/aspnet:8.0
    WORKDIR /App
    COPY --from=build-env /App/out .
    ENTRYPOINT ["dotnet", "DotNet.Docker.dll"]
```

Docker Compose (Java)
```sh
    services:
  semeando-backend:
    container_name: semeando-backend
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8080:8080
```
Docker Compose (.NET)
```sh
    dotnet-service:
        container_name: dotnet-service
        build:
          context: COLOCAR NOME DO PROJETO
          dockerfile: Dockerfile
        ports:
          - "5000:80"
```
## Documentação da API (Java)
#### Usuário

**GET /usuarios:** Lista todos os usuários.  
**POST /usuarios:** Cria um novo usuário.  
**GET /usuarios/{id}:** Retorna os detalhes de um usuário específico.  
**PUT /usuarios/{id}:** Atualiza as informações de um usuário.  
**DELETE /usuarios/{id}:** Remove um usuário pelo ID.  

## 📝 Guia Rápido de Comandos Importantes
**Clonar o Repositório (Java):**

    https://github.com/rbll0/semeando-spring.git
    cd semando-spring
    
**Clonar o Repositório (.Net):**

    https://github.com/rbll0/semeando-spring.git
    cd semando-spring

**Instalar Docker e Docker Compose:**

    sudo apt update
    sudo apt install -y ca-certificates curl gnupg lsb-release
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo apt update
    sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

**Configurar Docker Compose:**

    sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose

**Build das Imagens Docker:**

    cd semeando-cs (COLOCAR O CAMINHO CERTO)
    docker build -t (COLOCAR O NOME CERTO DO CONTAINER .NET) .
    cd semeando-spring
    docker build -t semeando-backend .

**Configuração e Deployment com Docker Compose:**

    docker-compose up --build -d
    docker-compose ps

**Teste a API:**

    curl http://<IP_DA_VM>:8080/usuarios


## Integrantes

| **Felipe Arcanjo** <br> ![Felipe](./Felipe.png) <br> **RM: 553472** | **Gustavo Rabelo** <br> ![Gustavo](./Gustavo.png) <br> **RM: 553481** | **Marcelo Vieira** <br> ![Marcelo](./Marcelo.png) <br> **RM: 553491** |
|:--:|:--:|:--:|
