 <p align="center">
  <img width="300" src="infobairrologo.png" alt="InfoBairro Logo" />
  
</p>

<h1 align="center">InfoBairro</h1>

<p align="center">
  <strong>Inteligência Urbana e Georreferenciamento</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-MVP-efd382?style=for-the-badge&logoColor=black" alt="Status">
  <img src="https://img.shields.io/badge/Framework-ASP.NET%20Core-8ab4d4?style=for-the-badge&logoColor=black" alt="Framework">
 
</p>


---

## 📝 1. Descrição do Projeto

O **InfoBairro** é uma plataforma Web Georreferenciada desenvolvida para empoderar os moradores de Camaçari. Através de um mapa interativo, a comunidade pode avaliar a infraestrutura (segurança, limpeza, mobilidade) de seus bairros, gerando dados reais que combatem a falta de informações atualizadas em canais oficiais.

---

## 🚀 2. Core Features (Funcionalidades)

- 🗺️ **Mapa Interativo:** Visualização georreferenciada de bairros e pontos de interesse.  
- ⭐ **Sistema de Ratings:** Avaliações de 1 a 5 estrelas em categorias críticas de infraestrutura.  
- 🛡️ **Painel de Moderação:** Interface administrativa para curadoria de conteúdo e segurança da plataforma.  
- 📊 **Data Intelligence:** Transformação de relatos em relatórios de tendência para o setor imobiliário e comercial.  
- 👤 **Gestão de Perfis:** Cadastro seguro com autenticação e recuperação de senha.  

---

## 💻 3. Tech Stack (Tecnologias)

- **Backend:** C# | ASP.NET Core MVC  
- **Frontend:** Razor Pages, Bootstrap 5, JavaScript  
- **Banco de Dados:** MariaDB (Relacional) com Entity Framework Core  
- **Arquitetura:** MVC (Model-View-Controller)  
- **Segurança:** Criptografia de senhas (Hash) e protocolo HTTPS  

---

## 🎲 4. Banco de Dados

A estrutura do banco de dados é responsável por organizar as informações do sistema 
e permitir que eles se relacionam de de forma eficiente. A seguir, é apresentada a organização 
e a relação entre suas informações.

  ### 💡 Visão geral
  O banco de dados do InfoBairro segue o modelo relacional, onde as informações são organizadas em tabelas conectadas entre si.

  Exemplificação:
  - Cidades -> agrupam bairros 
  - Bairros -> possuem informações e indicadores gerais
  - Avaliações -> representam a opinião dos usuários sobre os bairros 

  ### 🧠 Analogia simples 
  
Funciona como um sistema de avaliações:

  - A cidade organiza os locais em uma região maior
  - Cada bairro representa um lugar que pode ser analisado
  - As avaliações são as experiências e notas dadas pelo usuários

Com isso, o sistema consegue transformar opiniões individuais em uma visão geral sobre cada bairro.

### 🪢 Relacionamentos (DER)

- Uma cidade possui vários bairros (1:N)
- Um bairro pode ter várias avaliações (1:N)

📌 Exemplo:

``` Cidade -> vários Bairros -> várias Avaliações ```
### ⚠️ Observação importante (regra do sistema)

As notas não ficam armazenadas prontas na tabela de bairros. Elas são calculadas dinamicamente no sistema (backend).

Isso acontece porque:
- Evita inconsistência de dados
- Garante que as médias estejam sempre atualizadas
- Melhora a lógica de negócio

✔️ Ou seja:
As notas são calculadas a partir da tabela de avaliações.

---
## 🚀 5. Implantação do Banco de Dados

A implantação do banco de dados do **InfoBairro** é realizada de forma automatizada através do **Entity Framework Core**, garantindo que o ambiente possa ser reproduzido do zero em uma máquina limpa.

O sistema utiliza **migrações automáticas** e, ao iniciar a aplicação, verifica se o banco já existe. Caso não exista, ele é criado automaticamente, juntamente com todas as tabelas e dados iniciais necessários.

---

### 📌 Pré-requisitos

Antes da implantação, é necessário ter instalado:

- **.NET SDK 8+**
- **MySQL 8+ ou MariaDB 10+**
- **Git**
- **Entity Framework Core CLI** (opcional)

Para instalar a CLI:

```bash
dotnet tool install --global dotnet-ef
```

---

### 🔹 Passo 1: Clonar o repositório

```bash
git clone https://github.com/seu-repositorio/InfoBairro.git
cd InfoBairro
```

---

### 🔹 Passo 2: Configurar a conexão com o banco

No arquivo `appsettings.json`, preencher a chave `DefaultConnection`:

```json
"ConnectionStrings": {
  "DefaultConnection": "server=localhost;database=infobairro;user=root;password=sua_senha;port=3306"
}
```

> 💡 Em ambiente de produção, recomenda-se utilizar variáveis de ambiente para proteger as credenciais.

---

### 🔹 Passo 3: Executar a aplicação

```bash
dotnet run
```

Ao iniciar, o sistema executa automaticamente a migração do banco através do seguinte trecho presente no código:

```csharp
await context.Database.MigrateAsync();
```

Esse processo realiza:

- criação automática do banco, caso não exista
- criação de todas as tabelas
- aplicação das migrations pendentes
- atualização da estrutura do schema

---

### 🔹 Passo 4: Seed inicial do sistema

Após a criação do banco, o sistema executa a carga inicial de dados obrigatórios:

```csharp
await Seed.SeedRoles(roleManager);
await Seed.SeedMasterUser(userManager);
```

São criados automaticamente:

- perfis de acesso (**roles**)
- usuário administrador master
- permissões iniciais do sistema

Isso garante que o ambiente esteja pronto para uso logo após a primeira execução.

---

### ✅ Validação pós-implantação

Após a inicialização, validar se as tabelas foram criadas corretamente:

```sql
SHOW TABLES;
```

Resultado esperado (exemplo):

- `cidades`
- `bairros`
- `avaliacoes`
- `comentarios`
- `comentariolikes`
- `aspnetusers`
- `aspnetroles`

Também é recomendado validar a existência do usuário master e das roles iniciais.

---

### 🔍 Teste de funcionamento

Executar consultas simples para validar a integridade:

```sql
SELECT * FROM cidades;
SELECT * FROM bairros;
SELECT * FROM aspnetroles;
```

---

### 🔄 Rollback / Limpeza

Caso seja necessário desfazer a implantação:

#### Voltar para migration anterior

```bash
dotnet ef database update NomeMigrationAnterior
```

#### Remover banco completamente

```sql
DROP DATABASE infobairro;
```

---

### 🛡️ Observação técnica

O sistema utiliza:

- **retry automático de conexão**
- **migrations automáticas**
- **seed inicial**
- **exclusão em cascata**
- **índices únicos**

Essas práticas garantem maior confiabilidade na implantação e manutenção do ambiente.

---

## ✅ 6 Validação Pós-Implantação

Após a implantação do banco de dados, foi realizado um processo de validação para garantir que toda a estrutura, os relacionamentos e os dados iniciais do sistema foram criados corretamente.

Essa etapa é fundamental para assegurar que o ambiente está pronto para uso e que a aplicação consegue persistir e consultar informações sem inconsistências.

---

### 🗄️ 6.1 Validação da Estrutura do Banco

A primeira etapa consiste em verificar se todas as tabelas foram criadas com sucesso após a execução das migrations automáticas.

Execute no MySQL / MariaDB:

```sql
SHOW TABLES;
```

### 📌 Resultado esperado

O banco deve conter, no mínimo, as seguintes tabelas principais:

- `cidades`
- `bairros`
- `avaliacoes`
- `comentarios`
- `comentariolikes`
- `emailverificationcodes`
- `aspnetusers`
- `aspnetroles`
- `aspnetuserroles`

> 💡 As tabelas `aspnet*` são geradas automaticamente pelo **ASP.NET Identity**, responsável pelo sistema de autenticação e autorização.

---

### 👤 6.2 Validação do Seed Inicial

Durante a inicialização da aplicação, o sistema executa automaticamente a carga inicial de dados através dos métodos:

```csharp
await Seed.SeedRoles(roleManager);
await Seed.SeedMasterUser(userManager);
```

Essa etapa garante que o sistema já possua os perfis de acesso e o usuário administrador master.

Para validar, execute:

```sql
SELECT * FROM aspnetroles;
SELECT * FROM aspnetusers;
```

### 📌 Verificar

- existência dos perfis de acesso
- existência do usuário administrador
- e-mail e permissões iniciais

Exemplo esperado:

- `Admin`
- `Moderador`
- `Usuario`

---

### 🔗 6.3 Validação de Integridade Referencial

O banco utiliza **chaves estrangeiras e exclusão em cascata**, garantindo que os relacionamentos entre tabelas permaneçam consistentes.

Validar os principais relacionamentos:

```sql
SELECT * FROM cidades;
SELECT * FROM bairros;
SELECT * FROM avaliacoes;
SELECT * FROM comentarios;
```

### 📌 Conferir

- cada bairro deve possuir `CidadeId`
- cada avaliação deve possuir `Idbairro` e `IdUser`
- cada comentário deve possuir `BairroId` e `UsuarioId`

Essa etapa confirma que os relacionamentos definidos no `Context.cs` foram aplicados corretamente.

---

### 🌐 6.4 Validação Funcional da Aplicação

Após a validação estrutural, foram realizados testes diretamente na interface do sistema para comprovar a persistência dos dados.

### 📌 Testes executados

- cadastro de usuário
- login e autenticação
- criação de avaliação
- envio de comentário
- carregamento do mapa interativo
- validação de likes em comentários

Após cada operação, os dados foram conferidos no banco por meio de consultas SQL.

Exemplo:

```sql
SELECT * FROM avaliacoes;
SELECT * FROM comentarios;
```

---

### ⚡ 6.5 Validação de Regras de Negócio

Também foram validadas regras críticas implementadas no banco e na aplicação:

- não permitir duplicidade de likes por usuário no mesmo comentário
- impedir duplicidade de bairros na mesma cidade
- exclusão em cascata de registros relacionados

Exemplo de teste:

```sql
SELECT * FROM comentariolikes;
```

> 💡 O sistema possui índice único para impedir múltiplos likes do mesmo usuário no mesmo comentário.

---

### 🛡️ 6.7 Evidência de Teste

A validação pós-implantação foi executada em ambiente local pela equipe após a aplicação automática das migrations, confirmando que o banco está funcional e pronto para uso no MVP.

Todos os testes apresentaram resultado satisfatório.
---

## 🔄 7. Rollback / Limpeza do Banco de Dados

O rollback do banco pode ser realizado por meio das **migrations versionadas do Entity Framework Core**, garantindo reversão segura para versões anteriores da estrutura.

### ⏪ Reverter para migration anterior

```bash
dotnet ef migrations list
dotnet ef database update NomeMigrationAnterior
```

### 🗑️ Limpeza total

```sql
DROP DATABASE infobairro;
```

### 💾 Backup Preventivo (Recomendado)

Procedimento recomendado para backup preventivo:

```bash
mysqldump -u root -p infobairro > backup_infobairro.sql
```

Para restauração:

```bash
mysql -u root -p infobairro < backup_infobairro.sql
```
---



## 📈 8. Roadmap & Project Status

O projeto encontra-se atualmente em fase de **MVP (Mínimo Produto Viável)**.

- [x] **Phase 1 (2025.1):** Design (Figma), Modelagem de Dados (Diagramas ER) e Proposta de Valor  
- [x] **Phase 2 (2025.2):** Desenvolvimento do Core (Back-end, Front-end e Integração com DB)  
- [ ] **Phase 3 (2026.1):** Refinamento de código, PM Canvas e preparação para o Pitch Final *(Status Atual 🛠️)*  
- [ ] **Futuro:** Implementação de IA para análise de sentimento e Aplicativo Mobile nativo

 ---

## 🌐 9. Disponibilidade e Acesso

O **InfoBairro** é uma plataforma web de acesso público. Por se tratar de um sistema proprietário voltado à gestão urbana de Camaçari, o código-fonte reside em um repositório privado, enquanto a aplicação está disponível para uso da comunidade.

### 🔗 Link de Acesso
O sistema pode ser acessado através do link oficial:
> **https://infobairro.runasp.net/**

### 📱 Experiência do Usuário
* **Web Responsiva:** Otimizado para navegadores Desktop e Mobile (Chrome, Edge, Safari).
* **Sem Necessidade de Instalação:** Acesso direto via navegador, sem ocupar espaço no dispositivo.
* **Mapa em Tempo Real:** Carregamento dinâmico de dados georreferenciados via infraestrutura Cloud.

---

## 🛠️ 10. Infraestrutura e Deployment (Bastidores)

Embora o código seja privado, a arquitetura de publicação segue padrões modernos de escalabilidade:

* **Hospedagem:** Servidor Cloud configurado para alta disponibilidade (Uptime de 99%).
* **Segurança:** Camada de proteção SSL (HTTPS) para navegação segura dos usuários.
* **Banco de Dados:** Instância MariaDB dedicada para armazenamento seguro e anonimizado.
* **Integração:** Pipeline de deploy automatizado para atualizações de melhorias (Roadmap 2026).

  ---
## 👥 11. Equipe e Engenharia de Software

O **InfoBairro** é o resultado da colaboração estratégica entre especialistas em diferentes frentes de desenvolvimento.

### 🛠️ Core Team & Responsabilidades

| Membro | Role (Papel Técnico) | Especialidades no Projeto |
| :--- | :--- | :--- |
| **Gabriel Oliveira** | **Team Lead & Full Stack Developer** | Liderança de equipe, arquitetura ponta a ponta e gestão de entregas. |
| **Enya Arruda** | **BI Analyst & Frontend Engineer** | Análise de dados, implementação de UI e documentação técnica. |
| **Nicolly Brito** | **UI/UX Designer & Frontend** | Design de interface, experiência do usuário e documentação técnica. |
| **Arthur Michelângelo** | **Data Architect & Documentation** | Modelagem e alimentação de banco de dados, documentação e frontend. |
| **Leandro Rivas** | **Full Stack Developer & Designer** | Desenvolvimento de regras de negócio (Back), UI Design e Frontend. |

### 🏛️ Parcerias e Apoio Técnico
* **SENAI Camaçari:** Suporte em infraestrutura de laboratórios e orientação acadêmica.
* **Corpo Docente:** Mentoria técnica especializada em arquitetura .NET e Governança de Dados.

---


<p align="center">
  <sub><strong>Projeto desenvolvido no SENAI Camaçari • 2026</strong></sub><br>
  <sub><font size="1">Arthur Michelângelo • Enya Arruda • Gabriel Oliveira • Leandro Rivas • Nicolly Brito</font></sub>
</p>
