<!-- PROJECT_METADATA
{
  "title": "Sua Barbearia — Backend",
  "short_description": "API REST completa para gestão de barbearias: agendamentos, autenticação JWT, controle de horários, avaliações e módulo financeiro.",
  "primary_stack": ["Java", "Spring Boot", "PostgreSQL", "Flyway", "JWT", "Docker"],
  "architecture": "REST API",
  "detail_description": "Backend de uma plataforma completa de gestão para barbearias, desenvolvido com Java e Spring Boot seguindo princípios de Clean Architecture. Expõe uma API REST com múltiplos domínios: autenticação e autorização por perfil (cliente/profissional/estabelecimento) com JWT, agendamentos com validação de disponibilidade e bloqueio de horários, cadastro e vinculação de profissionais e serviços, módulo de avaliações e módulo financeiro. Usa PostgreSQL com Flyway para migrações versionadas e Docker Compose para ambiente reproduzível.",
  "images": [],
  "cover_image": "",
  "live_url": ""
}
-->

# Sua Barbearia — Backend

API REST para gestão completa de barbearias: agendamentos online, controle de profissionais e serviços, módulo financeiro e avaliações.

## Domínios da API

| Controller | Responsabilidade |
|---|---|
| `AuthController` | Autenticação JWT por perfil (cliente / profissional / barbearia) |
| `AgendamentoController` | CRUD de agendamentos com validação de disponibilidade |
| `HorarioController` | Gestão de horários disponíveis |
| `HorarioBloqueioController` | Bloqueio de horários (folgas, indisponibilidades) |
| `FuncionarioController` | Cadastro e gestão de profissionais |
| `ServicoController` | Cadastro e gestão de serviços oferecidos |
| `ClienteController` | Gestão de perfil de clientes |
| `BarbeariaController` | Gestão do estabelecimento |
| `AvaliacaoController` | Sistema de avaliações |
| `FinanceiroController` | Módulo financeiro |
| `ProfissionalDashboardController` | Dados do painel do profissional |

## Stack Técnica

| Camada | Tecnologia |
|---|---|
| Linguagem | Java 17 |
| Framework | Spring Boot 3 |
| Banco de Dados | PostgreSQL |
| Migrações | Flyway |
| Autenticação | JWT (Spring Security) |
| Containerização | Docker + Docker Compose |
| Arquitetura | Clean Architecture (adapters / domain / infrastructure) |

## Estrutura do Projeto

```
src/main/java/com/barbearia/
├── adapters/
│   └── controllers/    # Endpoints REST
├── domain/             # Entidades e regras de negócio
└── infrastructure/
    ├── persistence/    # Repositórios JPA
    └── security/       # Configuração JWT e Spring Security
```

## Como Rodar com Docker

```bash
# Subir PostgreSQL + aplicação
docker-compose up -d
```

A API estará disponível em `http://localhost:8080`.

## Como Rodar Localmente

### Pré-requisitos
- Java 17+
- Maven 3.8+
- PostgreSQL rodando localmente

```bash
# Compilar e rodar
./mvnw spring-boot:run
```

> Flyway executará as migrações automaticamente na primeira inicialização.

## Projeto Relacionado

Frontend: [sua-barbearia-frontend](https://github.com/Wanjos-eng/sua-barbearia-frontend)
