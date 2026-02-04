## 🎯 RESUMO EXECUTIVO

- **Total de Classes Reais:** 20 classes
- **Enums:** 5 tipos
- **Exceptions:** 4 classes
- **Cobertura de Conceitos POO:** 100%

---

## 📋 INVENTÁRIO DE CLASSES

### 1️⃣ ENTIDADES DE DOMÍNIO (20 classes)

#### **HIERARQUIA DE USUÁRIOS**

##### 1. **Usuario** (Classe Abstrata Base)
**Localização:** `domain/entities/Usuario.java`

**Conceitos POO Aplicados:**
- ✅ **Abstração** - Classe abstrata que define o contrato comum para todos os usuários
- ✅ **Encapsulamento** - Atributos privados (id, nome, email, senha, telefone, role) com getters/setters
- ✅ **Herança** - Classe base para Cliente e Barbearia
- ✅ **Construtores Protegidos** - Apenas subclasses podem instanciar

**Atributos:**
- `Long id` - Identificador único
- `String nome` - Nome completo
- `String email` - Email único no sistema
- `String senha` - Senha hasheada
- `String telefone` - Contato
- `String role` - Papel no sistema
- `LocalDateTime dataCriacao`
- `LocalDateTime dataAtualizacao`

**Métodos:**
- `atualizarDataModificacao()` - Atualiza timestamp de modificação

---

##### 2. **Cliente** (Herda de Usuario)
**Localização:** `domain/entities/Cliente.java`

**Conceitos POO Aplicados:**
- ✅ **Herança** - Estende Usuario e reutiliza comportamentos comuns
- ✅ **Encapsulamento** - Lógica específica de clientes encapsulada
- ✅ **Polimorfismo (Sobrescrita)** - equals(), hashCode(), toString()
- ✅ **Responsabilidade Única** - Gerencia apenas informações de clientes
- ✅ **Composição** - Possui lista de agendamentosIds

**Atributos Específicos:**
- `List<Long> agendamentosIds` - Histórico de agendamentos
- `boolean ativo` - Status no sistema
- `static final String ROLE_CLIENTE = "CLIENTE"`

**Métodos de Negócio:**
- `possuiAgendamentos()` - Verifica se tem agendamentos
- `adicionarAgendamento(Long)` - Adiciona agendamento ao histórico
- `ativar()` - Ativa o cliente
- `desativar()` - Soft delete

**Sobrescritas:**
- `equals(Object)` - Compara por email
- `hashCode()` - Consistente com equals
- `toString()` - Representação para logs

---

##### 3. **Barbearia** (Herda de Usuario)
**Localização:** `domain/entities/Barbearia.java`

**Conceitos POO Aplicados:**
- ✅ **Herança** - Estende Usuario
- ✅ **Encapsulamento** - Atributos privados e métodos específicos
- ✅ **Responsabilidade Única** - Gerencia estabelecimentos
- ✅ **Composição** - Possui listas de servicosIds e agendamentosIds

**Atributos Específicos:**
- `String nomeFantasia` - Nome do estabelecimento
- `TipoDocumento tipoDocumento` - CPF ou CNPJ
- `String documento` - Número sem formatação
- `String endereco` - Localização
- `List<Long> servicosIds` - Catálogo de serviços
- `List<Long> agendamentosIds` - Histórico
- `boolean ativo`
- `static final String ROLE_BARBEARIA = "BARBEARIA"`

**Métodos de Negócio:**
- `possuiServicos()` / `possuiAgendamentos()` - Verificações
- `adicionarServico(Long)` / `adicionarAgendamento(Long)` - Gestão
- `ativar()` / `desativar()` - Controle de status
- `isCPF()` / `isCNPJ()` - Verificação de tipo de documento

---

#### **HIERARQUIA DE FUNCIONÁRIOS E PERFIS**

##### 4. **Funcionario** (Classe Concreta)
**Localização:** `domain/entities/Funcionario.java`

**Conceitos POO Aplicados:**
- ✅ **Composição** - "Tem um" Perfil (Strategy Pattern)
- ✅ **Encapsulamento** - Atributos privados e delegação
- ✅ **Polimorfismo** - Delega comportamentos ao perfil
- ✅ **Factory Method** - `criarPerfil()` cria instâncias baseadas no tipo
- ✅ **Sobrescrita** - equals(), hashCode(), toString()

**Atributos:**
- `Long id`
- `Long barbeariaId`
- `String nome, email, telefone`
- `boolean ativo`
- `LocalDateTime dataCriacao, dataAtualizacao`
- `TipoPerfil tipoPerfil` - Enum discriminador
- `transient Perfil perfil` - Objeto não persistido, criado sob demanda

**Métodos de Negócio:**
- `getProfissao()` - Delega para perfil
- `podeRealizarServico(Servico)` - Delega validação
- `calcularComissao(double)` - Delega cálculo
- `verificarAtivo()` - Validação de estado
- `ativar()` / `desativar()` - Gestão de status

**Factory Method:**
```java
private Perfil criarPerfil(TipoPerfil tipo) {
    switch (tipo) {
        case BARBEIRO: return new PerfilBarbeiro();
        case MANICURE: return new PerfilManicure();
        case ESTETICISTA: return new PerfilEsteticista();
        case COLORISTA: return new PerfilColorista();
    }
}
```

---

##### 5. **Perfil** (Interface - Contrato)
**Localização:** `domain/entities/Perfil.java`

**Conceitos POO Aplicados:**
- ✅ **Abstração** - Interface que define contrato para todos os perfis
- ✅ **Polimorfismo** - Métodos implementados de forma diferente
- ✅ **Strategy Pattern** - Diferentes estratégias de comportamento

**Métodos do Contrato:**
- `String getProfissao()` - Tipo de profissão
- `boolean podeRealizarServico(Servico)` - Validação de capacidade
- `double calcularComissao(double)` - Cálculo específico (default 0.0)
- `String getEspecialidades()` - Descrição (default "Serviços gerais")

---

##### 6. **PerfilBarbeiro** (Implementa Perfil)
**Localização:** `domain/entities/PerfilBarbeiro.java`

**Conceitos POO Aplicados:**
- ✅ **Polimorfismo** - Sobrescreve métodos da interface
- ✅ **Encapsulamento** - Lógica específica interna
- ✅ **Sobrescrita** - toString()

**Particularidades:**
- Comissão: **15%**
- Serviços: `ServicoBarba`, `ServicoCorte`
- Especialidades: "Cortes de cabelo e barba"

---

##### 7. **PerfilManicure** (Implementa Perfil)
**Localização:** `domain/entities/PerfilManicure.java`

**Conceitos POO Aplicados:**
- ✅ **Polimorfismo** - Implementação específica
- ✅ **Encapsulamento** - Regras de manicure

**Particularidades:**
- Comissão: **12%**
- Serviços: `ServicoManicure`
- Especialidades: "Manicure e pedicure"

---

##### 8. **PerfilEsteticista** (Implementa Perfil)
**Localização:** `domain/entities/PerfilEsteticista.java`

**Conceitos POO Aplicados:**
- ✅ **Polimorfismo** - Implementação específica
- ✅ **Encapsulamento** - Regras de estética

**Particularidades:**
- Comissão: **13%**
- Serviços: `ServicoSobrancelha`
- Especialidades: "Design de sobrancelhas e estética facial"

---

##### 9. **PerfilColorista** (Implementa Perfil)
**Localização:** `domain/entities/PerfilColorista.java`

**Conceitos POO Aplicados:**
- ✅ **Polimorfismo** - Implementação específica
- ✅ **Encapsulamento** - Regras de coloração

**Particularidades:**
- Comissão: **18%** (maior por especialização)
- Serviços: `ServicoColoracao`, `ServicoTratamentoCapilar`
- Especialidades: "Coloração capilar e tratamentos químicos"

---

#### **HIERARQUIA DE SERVIÇOS**

##### 10. **Servico** (Classe Abstrata)
**Localização:** `domain/entities/Servico.java`

**Conceitos POO Aplicados:**
- ✅ **Abstração** - Define interface comum para serviços
- ✅ **Encapsulamento** - Atributos privados
- ✅ **Polimorfismo** - Método abstrato getTipoServico()
- ✅ **Template Method** - Estrutura comum, detalhes nas subclasses

**Atributos:**
- `Long id`
- `String nome, descricao`
- `BigDecimal preco`
- `Integer duracao` (minutos)
- `Long barbeariaId`
- `boolean ativo`
- `LocalDateTime dataCriacao, dataAtualizacao`

**Método Abstrato:**
- `abstract String getTipoServico()` - Cada subclasse define seu tipo

---

##### 11. **ServicoCorte** (Herda de Servico)
**Localização:** `domain/entities/ServicoCorte.java`

**Conceitos POO Aplicados:**
- ✅ **Herança** - Estende Servico
- ✅ **Polimorfismo** - Implementa getTipoServico()

**Tipo Retornado:** `"CORTE"`

---

##### 12. **ServicoBarba** (Herda de Servico)
**Localização:** `domain/entities/ServicoBarba.java`

**Conceitos POO Aplicados:**
- ✅ **Herança** - Estende Servico
- ✅ **Polimorfismo** - Implementa getTipoServico()

**Tipo Retornado:** `"BARBA"`

---

##### 13. **ServicoColoracao** (Herda de Servico)
**Localização:** `domain/entities/ServicoColoracao.java`

**Conceitos POO Aplicados:**
- ✅ **Herança** - Estende Servico
- ✅ **Polimorfismo** - Implementa getTipoServico()

**Tipo Retornado:** `"COLORACAO"`

---

##### 14. **ServicoManicure** (Herda de Servico)
**Localização:** `domain/entities/ServicoManicure.java`

**Conceitos POO Aplicados:**
- ✅ **Herança** - Estende Servico
- ✅ **Polimorfismo** - Implementa getTipoServico()

**Tipo Retornado:** `"MANICURE"`

---

##### 15. **ServicoSobrancelha** (Herda de Servico)
**Localização:** `domain/entities/ServicoSobrancelha.java`

**Conceitos POO Aplicados:**
- ✅ **Herança** - Estende Servico
- ✅ **Polimorfismo** - Implementa getTipoServico()

**Tipo Retornado:** `"SOBRANCELHA"`

---

##### 16. **ServicoTratamentoCapilar** (Herda de Servico)
**Localização:** `domain/entities/ServicoTratamentoCapilar.java`

**Conceitos POO Aplicados:**
- ✅ **Herança** - Estende Servico
- ✅ **Polimorfismo** - Implementa getTipoServico()

**Tipo Retornado:** `"TRATAMENTO_CAPILAR"`

---

#### **ENTIDADES DE GESTÃO**

##### 17. **Agendamento**
**Localização:** `domain/entities/Agendamento.java`

**Conceitos POO Aplicados:**
- ✅ **Encapsulamento** - Atributos privados, validações internas
- ✅ **Responsabilidade Única** - Gerencia apenas agendamentos
- ✅ **Máquina de Estados** - Transições de status controladas

**Atributos:**
- `Long id, clienteId, barbeariaId, barbeiroId, servicoId`
- `LocalDateTime dataHora`
- `StatusAgendamento status` - Enum de estados
- `String observacoes`
- `LocalDateTime dataCriacao, dataAtualizacao`

**Métodos de Negócio:**
- `confirmar()` - PENDENTE → CONFIRMADO
- `concluir()` - CONFIRMADO → CONCLUIDO
- `cancelar()` - Qualquer (exceto CONCLUIDO) → CANCELADO
- `atribuirBarbeiro(Long)` - Atribui profissional
- `ehPassado()` / `ehFuturo()` - Verificações de tempo

**Validações:**
- Cliente, barbearia e serviço obrigatórios
- Data/hora obrigatória
- Status inicial sempre PENDENTE
- Regras de transição de estados

---

##### 18. **HorarioFuncionamento**
**Localização:** `domain/entities/HorarioFuncionamento.java`

**Conceitos POO Aplicados:**
- ✅ **Encapsulamento** - Atributos privados
- ✅ **Responsabilidade Única** - Apenas horários
- ✅ **Validação de Regras de Negócio** - Consistência de dados

**Atributos:**
- `Long id, barbeariaId`
- `Integer diaSemana` (0=Domingo, 6=Sábado)
- `LocalTime horaAbertura, horaFechamento`
- `boolean ativo`

**Métodos de Negócio:**
- `validate()` - Valida consistência (abertura < fechamento)
- `sobrepoe(HorarioFuncionamento)` - Detecta conflitos de horário

---

##### 19. **FeriadoExcecao**
**Localização:** `domain/entities/FeriadoExcecao.java`

**Conceitos POO Aplicados:**
- ✅ **Encapsulamento** - Atributos privados
- ✅ **Validação de Regras de Negócio** - Lógica complexa
- ✅ **Responsabilidade Única** - Exceções de calendário

**Atributos:**
- `Long id, barbeariaId`
- `LocalDate data`
- `TipoExcecao tipo` - FECHADO ou HORARIO_ESPECIAL
- `LocalTime horarioAbertura, horarioFechamento`
- `String descricao`
- `boolean ativo`

**Métodos de Negócio:**
- `validate()` - Valida conforme tipo de exceção
- `estaAberto(LocalTime)` - Verifica disponibilidade

**Regras:**
- FECHADO: não pode ter horários definidos
- HORARIO_ESPECIAL: deve ter horários válidos

---

##### 20. **ProfissionalServico** (Tabela Pivô)
**Localização:** `domain/entities/ProfissionalServico.java`

**Conceitos POO Aplicados:**
- ✅ **Encapsulamento** - Atributos privados
- ✅ **Responsabilidade Única** - Relacionamento many-to-many

**Atributos:**
- `Long id, funcionarioId, servicoId`
- `boolean ativo`
- `LocalDateTime dataCriacao, dataAtualizacao`

**Finalidade:**
- Vincula Funcionário aos Serviços que pode executar
- Permite rastreabilidade temporal

---

## ENUMS (5 tipos)

### **TipoPerfil**
**Localização:** `domain/enums/TipoPerfil.java`

**Valores:**
- `BARBEIRO` - "Cortes de cabelo e barba"
- `MANICURE` - "Manicure e pedicure"
- `ESTETICISTA` - "Design de sobrancelhas e estética facial"
- `COLORISTA` - "Coloração capilar e tratamentos químicos"

**Atributos:**
- `String descricao`
- `String especialidades`

---

### **StatusAgendamento**
**Localização:** `domain/enums/StatusAgendamento.java`

**Valores:**
- `PENDENTE` - Aguardando confirmação
- `CONFIRMADO` - Confirmado pela barbearia
- `CONCLUIDO` - Serviço realizado
- `CANCELADO` - Cancelado

---

### **TipoDocumento**
**Localização:** `domain/enums/TipoDocumento.java`

**Valores:**
- `CPF` - Pessoa Física
- `CNPJ` - Pessoa Jurídica

---

### **TipoExcecao**
**Localização:** `domain/enums/TipoExcecao.java`

**Valores:**
- `FECHADO` - Fechamento total
- `HORARIO_ESPECIAL` - Horário diferenciado

---

### **PeriodoRelatorio**
**Localização:** `domain/enums/PeriodoRelatorio.java`

**Valores:** (para relatórios futuros)
- Diário, Semanal, Mensal, Anual

---

## EXCEPTIONS (4 classes)

### **AcessoNegadoException**
**Localização:** `domain/exceptions/AcessoNegadoException.java`

**Herança:** `RuntimeException`  
**Uso:** HTTP 403 (Forbidden)

---

### **AgendamentoNaoEncontradoException**
**Localização:** `domain/exceptions/AgendamentoNaoEncontradoException.java`

**Herança:** `RuntimeException`  
**Uso:** HTTP 404 (Not Found)

---

### **ClienteNaoEncontradoException**
**Localização:** `domain/exceptions/ClienteNaoEncontradoException.java`

**Herança:** `RuntimeException`  
**Uso:** HTTP 404 (Not Found)

---

### **ConflitoHorarioException**
**Localização:** `domain/exceptions/ConflitoHorarioException.java`

**Herança:** `RuntimeException`  
**Uso:** HTTP 409 (Conflict)

---
### **Hierarquias Identificadas**

#### **1. Hierarquia de Usuários**
```
Usuario (abstract)
├── Cliente
└── Barbearia
```

#### **2. Hierarquia de Serviços**
```
Servico (abstract)
├── ServicoCorte
├── ServicoBarba
├── ServicoColoracao
├── ServicoManicure
├── ServicoSobrancelha
└── ServicoTratamentoCapilar
```

#### **3. Composição de Perfis (Strategy)**
```
Perfil (interface)
├── PerfilBarbeiro
├── PerfilManicure
├── PerfilEsteticista
└── PerfilColorista
    ↑
    |
Funcionario (composição)
```

---

## PADRÕES DE DESIGN IDENTIFICADOS

### **1. Strategy Pattern**
- **Onde:** Funcionario + Perfil
- **Benefício:** Flexibilidade para adicionar novos perfis sem modificar Funcionario

### **2. Factory Method**
- **Onde:** Funcionario.criarPerfil()
- **Benefício:** Centraliza criação de perfis baseado em enum

### **3. Template Method**
- **Onde:** Servico (classe abstrata com getTipoServico())
- **Benefício:** Estrutura comum, detalhes específicos nas subclasses

### **4. Value Object**
- **Onde:** Enums (TipoPerfil, StatusAgendamento, etc.)
- **Benefício:** Imutabilidade e type safety

---

## PRINCÍPIOS SOLID APLICADOS

### **S - Single Responsibility Principle**
✅ **100% das classes** - Cada classe tem uma responsabilidade única e bem definida

### **O - Open/Closed Principle**
✅ Hierarquias de Servico e Perfil permitem extensão sem modificação

### **L - Liskov Substitution Principle**
✅ Subclasses (Cliente, Barbearia, ServicoCorte, etc.) podem substituir suas bases

### **I - Interface Segregation Principle**
✅ Interface Perfil com métodos coesos e específicos

### **D - Dependency Inversion Principle**
✅ Funcionario depende de abstração (Perfil), não de implementações concretas

---

## ✅ CONCLUSÃO

O domain layer do projeto **Sua Barbearia** demonstra:

1. ✅ **Arquitetura Rica** - 20 classes de domínio com regras de negócio encapsuladas
2. ✅ **POO Sólido** - Aplicação consistente de encapsulamento, herança, polimorfismo e abstração
3. ✅ **Design Patterns** - Strategy, Factory Method, Template Method
4. ✅ **SOLID Principles** - Aderência aos 5 princípios
5. ✅ **Domain-Driven Design** - Regras de negócio no domínio, não espalhadas
6. ✅ **Separação de Responsabilidades** - Cada classe com propósito único e claro
7. ✅ **Flexibilidade** - Fácil extensão (novos perfis, serviços, tipos de usuário)
8. ✅ **Manutenibilidade** - Código organizado e bem documentado

**Este é um exemplo de domain layer bem estruturado, seguindo as melhores práticas de OOP e Clean Architecture.**

---
