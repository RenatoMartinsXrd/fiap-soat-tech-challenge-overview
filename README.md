# Tech Challenge - Fase 4 - Documentação Geral

Este repositório contém a visão consolidada da arquitetura, domínios, modelagens e infraestrutura do sistema da lanchonete digital.

A lanchonete, antes um negócio de bairro, está passando por um processo de expansão devido ao seu grande sucesso. 
No entanto, o crescimento acelerado trouxe desafios operacionais, especialmente no controle de pedidos. 
Sem um sistema digital, os pedidos eram anotados manualmente, o que gerava confusão entre os atendentes e a cozinha, 
além de atrasos e erros no atendimento.

Para resolver esse problema e garantir uma operação eficiente em escala, para o design de negócio utilizamos **DDD (Domain-Driven Design)**, e para o design de software foi desenvolvida uma aplicação moderna baseada em microsserviços, com a infraestrutura provisionada na AWS através do Terraform e deploy automatizado com Github Actions.

---

## 🧭 Visão Geral da Arquitetura

O sistema foi dividido em microsserviços independentes, cada um responsável por um contexto específico da aplicação:

- [Product API](https://github.com/RenatoMartinsXrd/fiap-soat-tech-challenge-product-api): Gerencia os produtos disponíveis
- [Customer API](https://github.com/RenatoMartinsXrd/fiap-soat-tech-challenge-customer-api): Gerencia os dados dos clientes
- [Order API](https://github.com/RenatoMartinsXrd/fiap-soat-tech-challenge-order-api): Responsável por registrar e acompanhar pedidos
- [Payment API](https://github.com/RenatoMartinsXrd/fiap-soat-tech-challenge-payment-api): Responsável pelo processo de pagamento

O provisionamento da infraestrutura na AWS é realizado pelo terraform:
- [Infra (Terraform)](https://github.com/dequevedo/fiap-soat-tech-challenge-fase-4-infra-terraform): Gerencia a infraestrutura

---

## 🧠 DDD - Domain-Driven Design

O projeto segue os princípios do **Domain-Driven Design (DDD)** para estruturar os domínios, subdomínios e agregados. O **Event Storing** e o **Domain Storytelling** foram usados para documentar e modelar os processos de negócios.

> **Link para o repositório do DDD**: [DDD - Miro](https://miro.com/app/board/uXjVKgdT-iU=/)

---

## 🧩 Design de Software

Este microserviço adota o padrão **Clean Architecture**, com foco em separação de responsabilidades e independência tecnológica.

**Principais camadas:**

- **core**: Entidades, regras de negócio, casos de uso e contratos (ports)
- **adapters**: Controladores e mapeadores que traduzem o mundo externo para o domínio
- **frameworks**: Implementações específicas (REST, JPA, integrações externas)
- **shared**: Configurações globais e utilitários comuns
  

## 📐 Stack da Solução

- **Tecnologias**
   - Java 21 + Spring Boot
   - Flyway
   - Jpa
   - Swagger
   - JUnit e Mockito
   - Jacoco

- **Design Patterns**
  - DDD (Domain-Driven Design) para modelagem do domínio de negócios
  - Padrão Clean Architecture
  - Microsserviços isolados por domínio
  - Comunicação preferencialmente via HTTP (REST)
  - Estratégia futura para eventos assíncronos com RabbitMQ ou Kafka

- **Infraestrutura**
  - Docker
  - Kubernetes
  - Terraform
  - AWS
    - RDS
    - AWS Dynamo
    - Lambda
    - Cognito
    - API Gateway
    - Load Balancer

---

## Arquitetura da Infraestrutura e Distrubuição da aplicação

![DiagramaInfraAws](https://github.com/user-attachments/assets/9cbe7a70-8885-4fe6-a101-3274576f52d1)

- Os usuários acessam a aplicação por meio do **AWS API Gateway**, com autenticação gerenciada pelo **Amazon Cognito**.
- O tráfego autenticado é direcionado para um **Internal Network Load Balancer**, que distribui as requisições entre os nós do cluster.
- O cluster é composto por múltiplos nós EC2 do tipo T3, gerenciados pelo **Amazon EKS**, distribuídos em diferentes zonas de disponibilidade para garantir alta disponibilidade.
- O escalonamento dos nós é feito automaticamente com base na carga de trabalho.
- Os microserviços da aplicação interage com um banco de dados **PostgreSQL** hospedado no **Amazon RDS** e **Amazon DynamoDB** para payment
- O processo de integração e entrega contínua (CI/CD) é realizado via **GitHub Actions**, que constrói a imagem da aplicação e publica no ECR.

---

## 🔒 Autenticação e Autorização

A autenticação é feita via **AWS Cognito**. Os usuários se autenticam e obtêm um **Access Token**, usado nas chamadas às APIs protegidas.
![image](https://github.com/user-attachments/assets/0644d8b6-0f49-416f-b320-1e4b97008d70)


## 🛠️ Execução Local dos Microsserviços

Cada microserviço possui seu próprio `README.md` com instruções para:

- Rodar com Docker Compose
- Acessar Swagger/OpenAPI
- Configurar PGAdmin ou Mongo Compass

---

## 🧪 Testes

Cada microserviço possui:

- Testes unitários com JUnit e Mockito
- Coverage mínimo de 80%

---

## 👥 Equipe

- Renato Martins - @RenatoMartinsXrd
- Daniel Quevedo - @dequevedo

---
