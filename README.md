# Voll.med API

API REST da aplicação Voll.med, construída com Spring Boot 3, Spring Data JPA, Flyway e MySQL.

## Visão geral

Esta aplicação expõe endpoints para gerenciamento de médicos, pacientes e autenticação de usuários. A estrutura do projeto está organizada por domínio e segue uma abordagem de API REST com validações e persistência em banco relacional.

## Tecnologias

- Java 21
- Spring Boot 3.4.3
- Spring Web
- Spring Data JPA
- Spring Validation
- Spring Security
- Flyway
- MySQL
- Maven

## Estrutura do projeto

```text
src/
├── main/
│   ├── java/
│   │   └── med/voll/api/
│   │       ├── controller/
│   │       ├── domain/
│   │       ├── dto/
│   │       └── infra/
│   └── resources/
│       ├── application.properties
│       └── db/migration/
└── test/
    └── java/
```

## Requisitos

- Java 21
- Maven 3.9+ ou uso do wrapper `./mvnw`
- MySQL em execução local
- Banco criado com nome `p4_back_2026`

## Configuração do banco

O arquivo `src/main/resources/application.properties` contém a configuração de conexão:

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/p4_back_2026
spring.datasource.username=root
spring.datasource.password=mysql
```

Se o seu ambiente tiver outra credencial ou nome de banco, ajuste esse arquivo antes de iniciar a aplicação.

## Executando a aplicação

### Com Maven

```bash
mvn spring-boot:run
```

### Com Maven Wrapper

```bash
./mvnw spring-boot:run
```

A aplicação será iniciada na porta padrão do Spring Boot:

```text
http://localhost:8080
```

## Autenticação

A API utiliza Spring Security em modo stateless, com autenticação via `AuthenticationManager`.

### Endpoint de login

```http
POST /login
Content-Type: application/json
```

Body exemplo:

```json
{
  "login": "usuario@example.com",
  "senha": "123456"
}
```

> Observação: nesta versão, o projeto autentica credenciais e retorna sucesso quando o `AuthenticationManager` valida o usuário, mas ainda não implementa geração de JWT ou autorização por roles avançadas.

## Endpoints principais

### Médicos

```http
GET    /medicos
GET    /medicos/{id}
POST   /medicos
PUT    /medicos
DELETE /medicos/{id}
```

#### Exemplo de cadastro

```http
POST /medicos
Content-Type: application/json
```

```json
{
  "nome": "Dr. João Silva",
  "email": "joao@voll.med",
  "crm": "123456",
  "telefone": "11999999999",
  "especialidade": "ORTOPEDIA",
  "endereco": {
    "logradouro": "Rua das Flores",
    "bairro": "Centro",
    "cep": "01000-000",
    "cidade": "São Paulo",
    "uf": "SP",
    "numero": "123",
    "complemento": "Apartamento 1"
  }
}
```

### Pacientes

```http
GET    /pacientes
POST   /pacientes
PUT    /pacientes
DELETE /pacientes/{id}
```

## Migrations

O projeto usa Flyway para versionar o esquema do banco. Os scripts ficam em:

```text
src/main/resources/db/migration/
```

Os arquivos existentes incluem criação das tabelas de médicos, pacientes e usuários, além de alterações nas tabelas existentes.

## Testes

Para rodar a suíte de testes:

```bash
mvn test
```

## Observações

- A API usa `@Transactional` em operações de escrita.
- O modelo de usuário implementa `UserDetails` do Spring Security.
- A aplicação pode ser usada como base para desenvolvimento de uma clínica/consultório digital com autenticação e gerenciamento de cadastro de profissionais e pacientes.

## Licença

Este projeto foi desenvolvido como base de estudo e demonstração de uma API REST com Spring Boot.
