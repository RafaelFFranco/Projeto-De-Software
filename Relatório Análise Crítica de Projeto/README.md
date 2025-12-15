# 📘 Relatório de Análise Crítica do Projeto 👨‍💻

## 1. Informações do grupo
- **🎓 Curso:** Engenharia de Software
- **📘 Disciplina:** Laboratório de Desenvolvimento de Software
- **🗓 Período:** 4° Período
- **👨‍🏫 Professor(a):** Prof. Dr. João Paulo Carneiro Aramuni
- 👥 Membros do Grupo: **Rafael Franco**

---

## 📌 2. Identificação do Projeto
- **Nome do projeto:** Sistema de Moeda Estudantil
- **Integrantes do outro grupo:** Leonardo Viana, Paulo Assis, Pedro Maia
- **Link do repositório:** _https://github.com/VianaLeo13/Laboratorio-Desenvolvimento-Software/tree/main/Laboratorio_03_
- **Pull requests submetidos pelo seu grupo:**

| 👤 Integrante | 🔧 Refatoração | 🔗 Link do PR |
|--------------|---------------|----------------|
|  <a href="#">Rafael Franco</a> | Segurança e DTO (AlunoResponse), Criação de Mapper, Arquitetura de Camadas (Controller/Service) | https://github.com/VianaLeo13/Laboratorio-Desenvolvimento-Software/pull/24/files#diff-48083a2b01a1a3709cf24e5a5ac99372fb00520cac087804e8a9b6c1833a01cb |


### 📝 Abrindo o Pull Request: Fluxos de Contribuição via PRs

#### 1. Opção 1 — Usando *Fork* (quando você **não é colaborador**)

1. Crie um **fork** (cópia) do repositório do outro grupo na sua conta.
2. **Clone o seu fork** localmente.
3. Crie um **branch**, faça as refatorações (commits) e envie (**push**) a branch para o seu fork.
4. No GitHub, acesse o **seu fork** e inicie o pull request clicando em **"Compare & pull request"**.
5. O PR deve propor mesclar o **seu branch** para a branch **main** do **repositório original** do outro grupo.
6. Adicione título/descrição e clique em **"Create pull request"**.

#### 2. Opção 2 — Como Membro/Colaborador (quando você **foi incluído** no repositório)

1. Peça para o outro grupo **adicionar seu usuário GitHub como colaborador**.
2. **Clone o repositório original** localmente.
3. Crie um **branch**, faça as refatorações (commits) e envie (**push**) a branch diretamente para o repositório original.
4. No GitHub, no repositório original, inicie o pull request clicando em **"Compare & pull request"**.
5. O PR deve propor mesclar o **seu branch** para a branch **main** do **mesmo repositório**.
6. Adicione título/descrição e clique em **"Create pull request"**.

---

## 🧱 3. Arquitetura e Tecnologias Utilizadas

O projeto "Sistema de Moeda Estudantil" visa criar um ecossistema de gamificação educacional. A arquitetura segue o padrão de camadas para separar a interface (Front) das regras de negócio (Back).

### 🏗️ Backend
O backend foi estruturado para gerenciar as entidades principais: `Aluno`, `Professor`, `Empresa` e `Transacao`.
* **Framework:** O código refatorado utiliza **Micronaut Framework 4.0** 

* **Persistência:** Uso de JPA/Hibernate para mapeamento objeto-relacional (compatível com PostgreSQL ou H2 usado para testes).

### 🎨 Views e Frontend
* **Tecnologia:** HTML5, CSS3 e JavaScript (Vanilla).
* **Comunicação:** O frontend consome a API RESTful do backend via `fetch` API.
* **Estrutura:** O frontend é desacoplado, servido estaticamente, comunicando-se com o backend via JSON.

### 🔄 Integração entre Camadas
A refatoração focou em corrigir o fluxo de dados:
1.  **Controller:** Recebe requisição HTTP (ex: Cadastro de Aluno).
2.  **Service:** Valida regras (ex: CPF único) e usa o **Mapper**.
3.  **Repository:** Executa a query no banco.
4.  **DTO:** Retorna apenas os dados seguros para o Frontend.

---

## 🗂️ 4. Organização do GitHub e Fluxo de Trabalho Colaborativo

### 4.1. Estrutura do Repositório e Documentação
* **Pontos Positivos:** O repositório possui uma estrutura de pastas organizada (`frontend`, `moedas_micronaut`, `docs`) e inclui diagramas UML (Caso de Uso, Sequência, Entidade-Relacionamento) diretamente no README.
* **Inconsistência Identificada:** O arquivo `README.md` é visualmente rico, mas contraditório. Ele cita **Spring Boot** na introdução ("Desenvolvido com arquitetura moderna usando Spring Boot"), mas exibe badges e seções técnicas afirmando usar **Micronaut**. Isso gera confusão para novos desenvolvedores configurando o ambiente.

### 4.2. Gerenciamento de Tarefas
* A organização do projeto faz o uso de metodologias ágeis, dada a presença de Histórias de Usuário (HS01 a HS05) bem definidas com Critérios de Aceite.

### 4.3. Padrões de Commits
* O projeto incentiva o uso de **Conventional Commits** (citado na seção "Contribuição"), o que facilita a geração de logs de alteração e rastreabilidade das refatorações propostas.

---

## 🖥️ 5. Dificuldade para Configuração do Ambiente

### 5.1. Conflito de Documentação (Spring vs Micronaut)
A principal dificuldade encontrada foi determinar o framework correto para o *build*.
* **Problema:** O desenvolvedor pode tentar rodar `mvn mn:run` (comando Micronaut) em um projeto que estruturalmente parece Spring Boot, ou vice-versa.
* **Solução:** Foi necessário inspecionar o `pom.xml` para confirmar as dependências reais antes de iniciar a aplicação.

### 5.2. Banco de Dados e Variáveis
* O sistema depende de variáveis de ambiente para conexão com o banco (`DATASOURCES_DEFAULT_URL`).
* **Ajuste:** Para facilitar o teste local, configuramos o perfil `dev` para usar o H2 em memória automaticamente, sem necessidade de configuração externa complexa.

---

## 🔎 6. Análise de Qualidade do Código e Testes

### 6.1. Design e Princípios SOLID
* **Acoplamento (Antes):** A `AlunoController` estava injetando diretamente o `AlunoRepository`. Isso violava a separação de camadas, expondo métodos de banco de dados (como `deleteAll`) diretamente na API.
* **Coesão (Antes):** A conversão de `Aluno` para JSON ocorria misturada com a regra de negócio, dificultando a leitura e ferindo o princípio de responsabilidade única.

### 6.2. Segurança (OWASP - Data Exposure)
* **Vulnerabilidade:** O endpoint `GET /alunos/{id}` retornava a entidade JPA completa. Se a entidade `Aluno` tivesse campos como `senha` ou `dadosAudit`, eles seriam vazados.
* **Correção:** Implementação do padrão **DTO (Data Transfer Object)** através do record `AlunoResponse`, garantindo que apenas dados públicos sejam enviados.

### 6.3. Testabilidade
* A ausência de injeção de dependência via construtor (uso excessivo de `@Autowired` em campos privados) dificultava testes unitários. A refatoração para Services facilita o uso de Mocks (Mockito).

---

## 🚀 7. Sugestões de Melhorias

1.  **Unificar Documentação:** Corrigir o README para decidir entre Spring Boot ou Micronaut, eliminando a ambiguidade atual.
2.  **Segurança de Transações:** Implementar verificação de atomicidade (`@Transactional`) no método de transferência de moedas entre Professor e Aluno para evitar saldos inconsistentes em caso de erro.
3.  **Validação de Input:** Adicionar Bean Validation (`@NotNull`, `@Min(1)`) nos DTOs de envio de moedas para impedir valores negativos ou nulos.
4.  **Testes de Integração:** Criar testes automatizados para o fluxo crítico: Professor envia moeda -> Saldo Aluno aumenta -> Email disparado.
5.  **Criptografia:** Garantir que senhas de alunos e empresas sejam hashadas (com BCrypt) antes da persistência 

---

## 🔧 8. Refatorações Propostas (3 partes do código)

### 1️⃣ Refatoração 1 – Segurança com DTO (Data Transfer Object)

**Arquivo:** `src/main/java/com/moedas/controller/AlunoController.java`

#### 🔴 Antes (Retorno de Entidade Completa)
```java
@Builder
@Data
@NoArgsConstructor
@AllArgsConstructor
@Serdeable
public class CreateAlunoResponseDTO {

    private long id;
    private String cpf;
    private String rg;
    private String nome;
    private String email;
    private String endereco;
    private String senha
}
```

#### 🟢 Depois (Uso de DTO Seguro)
```java
@Builder
@Data
@NoArgsConstructor
@AllArgsConstructor
@Serdeable
public class CreateAlunoResponseDTO {

    private long id;
    private String cpf;
    private String rg;
    private String nome;
    private String email;
    private String endereco;
    //Remove senha do DTO de response por segunraçança
}
```

### 2️⃣ Refatoração 2 – Coesão com Mapper
**Arquivo:** `src/main/java/com/moedas/mapper/AlunoMapper.java`

#### 🔴 Antes (Métodos de conversão DTO->Entidade e Entidade->DTO na classe service)
```java
    private Aluno createEntity(CreateAlunoRequestDTO createAlunoRequestDTO) {
        return Aluno.builder()
                .nome(createAlunoRequestDTO.getNome())
                .cpf(createAlunoRequestDTO.getCpf())
                .email(createAlunoRequestDTO.getEmail())
                .senha(createAlunoRequestDTO.getSenha())
                .rg(createAlunoRequestDTO.getRg())
                .endereco(createAlunoRequestDTO.getEndereco())
                .build();
    }

    private CreateAlunoResponseDTO createDTO(Aluno aluno) {
        return CreateAlunoResponseDTO.builder()
                .id(aluno.getId())
                .email(aluno.getEmail())
                .senha(aluno.getSenha())
                .nome(aluno.getNome())
                .endereco(aluno.getEndereco())
                .rg(aluno.getRg())
                .cpf(aluno.getCpf())
                .build();
    }
}
```

#### 🟢 Depois (Uso de Mapper Dedicado)
```java
//Classe mapper para centralizar conversões entre DTOs e entidades
public final class AlunoMapper {

    private AlunoMapper() {}

    public static Aluno toEntity(CreateAlunoRequestDTO dto) {
        return Aluno.builder()
                .nome(dto.getNome())
                .cpf(dto.getCpf())
                .email(dto.getEmail())
                .senha(dto.getSenha())
                .rg(dto.getRg())
                .endereco(dto.getEndereco())
                .build();
    }

    public static CreateAlunoResponseDTO toDto(Aluno aluno) {
        return CreateAlunoResponseDTO.builder()
                .id(aluno.getId())
                .email(aluno.getEmail())
                .nome(aluno.getNome())
                .endereco(aluno.getEndereco())
                .rg(aluno.getRg())
                .cpf(aluno.getCpf())
                .build();
    }
}
```


### 3️⃣ Refatoração 3 – Arquitetura de Camadas (Dependency Injection)
#### 🔴 Antes (Violação de Camada)

```java
@Secured(SecurityRule.IS_AUTHENTICATED)
@RequiredArgsConstructor
public class AlunoController {
    private final TransacaoRepository transacaoRepository;
    private final AlunoRepository alunoRepository;
    private final AlunoService alunoService;


    @Get("/{id}/extrato-transacoes") // 324 app.js
    @Secured(SecurityRule.IS_ANONYMOUS)
    public List<Transacao> getExtratoTransacoes(@PathVariable Long id) {
        return transacaoRepository.findByAlunoIdOrderByDataHoraDesc(id);
    }

    @Get("/{id}/saldo")
    @Secured(SecurityRule.IS_ANONYMOUS)
    public Double getSaldo(@PathVariable Long id) {
        Aluno aluno = alunoRepository.findById(id).orElseThrow(() -> new RuntimeException("Error"));
        return aluno.getSaldoMoedas();
    }
}
```

#### 🟢 Depois (Arquitetura Correta)
```java
@Secured(SecurityRule.IS_AUTHENTICATED)
@RequiredArgsConstructor
public class AlunoController {
    //Remove AlunoRepository e TransacaoRepository, usa apenas AlunoService
    private final AlunoService alunoService;


    @Get("/{id}/extrato-transacoes")
    @Secured(SecurityRule.IS_ANONYMOUS)
    public List<Transacao> getExtratoTransacoes(@PathVariable Long id) {
        return alunoService.getExtratoTransacoes(id);
    }

    @Get("/{id}/saldo")
    @Secured(SecurityRule.IS_ANONYMOUS)
    public Double getSaldo(@PathVariable Long id) {
        return alunoService.getSaldo(id);
    }
}
```

---

## 9. 📄 Conclusão

A análise do **Sistema de Moeda Estudantil** revelou um projeto com grande potencial de engajamento acadêmico e uma base documental visualmente forte (diagramas, histórias de usuário). No entanto, a implementação técnica inicial carecia de rigor na arquitetura, apresentando **acoplamento entre Controller e Banco de Dados** e **riscos de segurança na exposição de dados**.

As refatorações realizadas trouxeram o projeto para um nível profissional de Engenharia de Software:

1. **Segurança:** A introdução de **DTOs** blinda os dados sensíveis dos alunos.
2. **Organização:** A camada de **Mappers** limpa o código de negócio.
3. **Escalabilidade:** A separação correta das camadas permite que novas features (como integração com parceiros externos) sejam adicionadas sem quebrar a API existente.

Recomenda-se agora a correção urgente das inconsistências na documentação (definição do stack tecnológico) e a implementação de **testes automatizados** para garantir a confiabilidade das transações financeiras (moedas) do sistema.

---

## 10. 📚 Referências
- Patterns of Enterprise Application Architecture (Martin Fowler)
- Spring Boot & Layered Architecture Best Practices
- OWASP Top 10 API Security Risks (Data Exposure)
- GitHub Classroom & Conventional Commits Guide
