<!-- PROJECT_METADATA
{
  "title": "Sua Barbearia — Backend API",
  "short_description": "API REST Java/Spring Boot para gestão completa de barbearias: 13 controllers, autenticação JWT por perfil, Flyway para migrações e Docker Compose para deploy.",
  "primary_stack": ["Java", "Spring Boot", "PostgreSQL", "Spring Security", "JWT", "Flyway", "Docker"],
  "architecture": "REST API",
  "detail_description": "Backend de uma plataforma real de gestão para barbearias, desenvolvido com Java 17 e Spring Boot 3 seguindo Clean Architecture (adapters / domain / infrastructure). A autenticação usa Spring Security com JWT diferenciado por perfil — um cliente, profissional e estabelecimento recebem tokens com claims distintos, e os endpoints são protegidos com guards customizados por role. O módulo de agendamento é o mais complexo: valida disponibilidade em tempo real cruzando horários bloqueados (`HorarioBloqueio`), horários regulares e a duração do serviço escolhido, impedindo conflitos mesmo em requisições concorrentes via transações PostgreSQL com isolation level adequado. As migrações de banco são versionadas com Flyway, garantindo que cada ambiente (dev/staging/prod) evolua de forma rastreável e reversível. O ambiente é containerizado com Docker Compose — um comando sobe PostgreSQL + aplicação com health checks e restart policy.",
  "images": [],
  "cover_image": "",
  "live_url": ""
}
-->

# Sua Barbearia — Backend API

API REST de gestão completa para barbearias. 13 controllers, autenticação JWT por perfil de usuário, e validação de conflitos de agendamento em tempo real.

## Domínios da API

| Controller | Responsabilidade |
|---|---|
| `AuthController` | JWT por perfil: claims distintos para cliente/profissional/barbearia |
| `AgendamentoController` | CRUD com validação de conflitos + duração de serviço |
| `HorarioController` | Gestão de horários disponíveis |
| `HorarioBloqueioController` | Bloqueio de horários (folgas, indisponibilidades) |
| `FuncionarioController` | Cadastro e gestão de profissionais |
| `ServicoController` | Serviços com duração e preço |
| `ClienteController` | Perfil e histórico de clientes |
| `BarbeariaController` | Configuração do estabelecimento |
| `AvaliacaoController` | Sistema de avaliações pós-atendimento |
| `FinanceiroController` | Módulo financeiro |
| `ProfissionalDashboardController` | Dados em tempo real para painel do profissional |

## Stack Técnica

| Camada | Tecnologia |
|---|---|
| Linguagem | Java 17 |
| Framework | Spring Boot 3 |
| Segurança | Spring Security + JWT (claims por perfil) |
| Banco | PostgreSQL |
| Migrações | Flyway (versionadas e reversíveis) |
| Containerização | Docker + Docker Compose |
| Arquitetura | Clean Architecture |

## Como Rodar

```bash
docker-compose up -d   # Sobe PostgreSQL + app
```

## Projeto Relacionado

Frontend: [sua-barbearia-frontend](https://github.com/Wanjos-eng/sua-barbearia-frontend)
