# Jenkins + Jolokia em Kubernetes com KIND


Este repositório contém a configuração de uma imagem personalizada do Jenkins com suporte ao Jolokia, implantada em um cluster Kubernetes local utilizando o KIND (Kubernetes IN Docker).

## 🛠️ Requisitos

 -  Kind
 -  Kubectl
 -  Acesso à internet para baixar Jenkins e Jolokia
 -  Linux (ou compatível com Docker)

## 📦 Componentes


- [Jenkins](https://www.jenkins.io/)  (modo WAR)
- [Apache Tomcat 9](https://tomcat.apache.org/)
- [Jolokia Agent](https://jolokia.org/)
- [Kind](https://kind.sigs.k8s.io/)
- [Kubectl](https://kubernetes.io/docs/home/)

## 🖥️ Como executar

### 1. Clone o repositório

```
git clone https://github.com/michelwanderson/jenkins-tomcat-jolokia-k8s.git
```

### 2. Baixe o arquivo jenkins.war

```
wget -O jenkins.war  https://get.jenkins.io/war/2.440/jenkins.war
```

### 2. Construa a Imagem Docker
```
docker build -t k8s-jenkins-tomcat-jolokia .
```

### 3. Crie o cluster com KIND
```
kind create cluster --config config.yaml 
```

### 4. Construir e carregar a imagem no KIND
```
kind load docker-image --name cluster-jenkins-tom-jolokia
```

### 5. Aplicar o deployment no Kubernetes
```
kubectl apply -f deploy.yaml
```

### 6.  Verificar o funcionamento
```
kubectl get pods kubectl
```


## 🌐 Acessos
#### Uma das formas de acesso via navegador é realizando mapeamento de portas
```
port-forward svc/jenkins-tomcat-jolokia-service 8080:8080
```

-   **Jenkins Web:**  [http://localhost:8080/jenkins](http://localhost:8080/jenkins)
-   **Jolokia API:**  [http://localhost:8080/jolokia](http://localhost:8080/jolokia)



## 🔍 Verificando métricas via Jolokia

Exemplo de chamada:

```
curl http://localhost:8080/jolokia/version
curl http://localhost:8080/jolokia/read/java.lang:type=Memory
```

# Abrir o Jenkins

Para garantir que o Jenkins está configurado de forma segura pelo administrador, uma senha foi escrita no arquivo de registro, para salva-la e inserir no primeiro acesso via  [http://localhost:8080/jenkins](http://localhost:8080/jenkins)

```
kubectl exec -it  cat root/.jenkins/secrets/initialAdminPassword
```

----------

## 🌟 Features

-   _Automação CI/CD:_  Jenkins instalado e configurado.
-   _Monitoramento JMX:_  Jolokia configurado para exposição via HTTP.
-   _Deploy em Kubernetes:_  Aplicação escalável, melhorando a confiabilidade

## 🔒 Considerações de Segurança



----------

## 🗂️ Estrutura do projeto