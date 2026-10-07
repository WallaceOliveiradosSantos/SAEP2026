**🚀 Guia Prático: SISTEMA DE GESTÃO DE CLÍNICA - AGENDAMENTO DE
CONSULTAS**

📌 Contexto do Projeto: Sistema de Gestão de Clínica e Agendamento de
Consultas A clínica precisa informatizar a abertura e gestão de
consultas médicas para melhorar a organização da fila de atendimento.
Atualmente, os pacientes ligam ou enviam e-mails à recepção para
solicitar atendimento de médicos de diversas especialidades (Ex:
Cardiologia, Ortopedia, Clínica Geral). No sistema, um Paciente cadastra
seus dados pessoais (Nome, Cpf, Telefone e Data de Nascimento). O Médico
possui seu cadastro contendo Nome, Crm e Especialidade e Especialidade.
Um Consulta representa a ordem de serviço em si, onde é registrado o
Paciente, o Médico responsável, a Data/Hora agendada e o Status do
atendimento (Ex: Agendada, Realizada, Cancelada). O paciente só consegue
fazer abertura de consulta se estiver logado. Ao abrir o sistema a
primeira tela é a de login, que deve conter abaixo também uma opção para
a pessoa fazer o cadastro, caso ainda não tenha.

**🛠️ Pré-requisitos Visual Studio 2022 (com suporte ao .NET 8 instalado)
Microsoft SQL Server e SQL Server Management Studio (SSMS)**

🗄️ Etapa 1: Levantamento de Requisitos Funcionais

Os requisitos funcionais estão atrelados ao negócio do usuário. Um
exemplo é: o paciente só pode realizar o agendamento de uma consulta se
estiver logado. Os requisitos funcionais são diferentes dos requisitos
não funcionais, pois os não funcionais referem-se, por exemplo, à
definição de qual banco de dados usar, linguagem de programação,
segurança ou desempenho.

Com base no contexto, podemos definir a seguinte lista inicial de
requisitos funcionais:

  -----------------------------------------------------------------------
  **ID**        **Requisito**                **Descrição**
  ------------- ---------------------------- ----------------------------
  R1            Cadastro de Pacientes        O sistema deve permitir
                                             cadastrar, visualizar,
                                             atualizar e excluir (CRUD)
                                             os dados dos pacientes.

  R2            Cadastro de Médicos          O sistema deve permitir
                                             cadastrar, visualizar,
                                             atualizar e excluir (CRUD)
                                             os dados dos médicos,
                                             incluindo CRM e
                                             Especialidade.

  R3            Agendamento de Consulta      O sistema deve permitir
                                             agendar consultas vinculando
                                             obrigatoriamente 1 Paciente
                                             e 1 Médico a uma data/hora
                                             específica.

  R4            Controle de Status           Toda consulta deve possuir
                                             um status, podendo ser
                                             Agendada, Realizada ou
                                             Cancelada.

  R5            Integridade Relacional       Não deve ser possível
                                             excluir um paciente ou
                                             médico que possua consultas
                                             vinculadas no histórico.

  R6            Autenticação do Paciente     O sistema deve permitir que
                                             o paciente acesse o sistema
                                             informando seu CPF.

  R7            Controle de Acesso           O paciente autenticado deve
                                             visualizar somente suas
                                             próprias consultas.

  R8            Vínculo Automático           Ao realizar um agendamento,
                                             o sistema deve vincular
                                             automaticamente a consulta
                                             ao paciente autenticado.
  -----------------------------------------------------------------------

**🗄️ Etapa 2: DER - DIAGRAMA DE ENTIDADE E RELACIONAMENTO **

Com base no contexto e requisitos podemos considerar a estrutura a
seguir (DER) como ponto de partida para implementar nosso sistema.

ATENÇÃO: No mercado de trabalho o importante é focar no pedido
solicitado. Se um gestor pede para implementar o DER acima, foque nessa
entrega. Não é interessante criar outras tabelas e campos sem antes
alinhar com a sua supervisão. Para criar o banco de dados você pode
fazer via Script, ou via interface do SQL Server Management. Vamos fazer
usando a proposta de interface (cliques, tela). Abra o SQL Server
Management e coloque as credenciais de login e senha.

Ao lado esquerdo para criar o banco de dados clique com o botão direito
em Banco de Dados e em seguida Novo Banco de Dados. Defina o nome do
banco de dados para **dbClinica** e depois confirme clicando em ok.

Para verificar se o banco foi criado, expanda a visualização de banco
clicando no sinal de + de banco de dados à esquerda e se ainda não
apareceu clique no botão atualizar (azul).

Para criar as tabelas vamos usar a opção visual Criar Diagrama de Banco
de Dados. Clique com o botão direito nessa opção estando na visualização
do banco **dbClinica** e em seguida clique em Novo Diagrama de Banco de
Dados.

Atenção: se aparecer alguma mensagem pode confirmar Sim, ou ok. Também,
deve aparecer uma tela para adicionar tabelas, mas como ainda não
existem tabelas criadas estará vazia. Basta clicar em fechar. Para criar
uma nova tabela clique com o botão direito em qualquer lugar e clique em
Nova Tabela. Em seguida vamos nomear essa tabela como **Paciente**.

Nomeie para **Paciente**. Defina os nomes dos campos e seus tipos
conforme a estrutura derivada do DER.

Observe que todos os campos são obrigatórios, por isso Permitir Nulos
está desmarcado para ele. No entanto, para o campo Codigo ele está
marcado. Não se preocupe, porque esse campo iremos gerar o código de
forma automática e sequencial. Sendo assim, ele nunca ficará nulo.
Atenção: Importante você salvar as alterações clicando em Salvar
("disquete"). Como é a primeira vez será solicitado o nome que você quer
ofertar ao Diagrama, coloque **DER Clinica**.

Chave Primária: O campo Codigo conforme contexto e DER será nossa chave
primária e para garantir sistematicamente que ele não se repita, clique
com o botão direito em cima de Codigo e em seguida clique em Definir
Chave Primária.

ATENÇÃO! Ops. Se depois que você clicar em salvar, aparecer uma janela
de alerta informando que não é possível salvar alterações, faça as
seguintes etapas: 8.2. Clique em ferramentas, opções. Na janela que for
aberta clique na opção Designers e desmarque as opções de aviso e
confirme. Clique novamente em salvar e verifique se o projeto foi salvo.

Observe que a chave amarela vai estar ao lado de código e o asterisco ao
lado do nome Paciente também sumiu confirmando que todas as alterações
foram salvas.

Código automático: Para que o código do Paciente seja inserido em
numeração automática. Você deve clicar com o botão direito em Codigo e
em seguida, clicar em Propriedades. Nas opções que abrir na parte
direita da tela deixe as opções a seguir marcadas com Sim e valor 1:

-   Especificação de Identidade = Sim
-   (É identidade) = Sim
-   Incremento de Identidade = 1
-   Semente de Identidade = 1

Crie as tabelas **Tecnico** e **Consulta** seguindo os passos feitos
para criarmos a tabela **Paciente**.

ATENÇÃO! A tabela **Consulta** vai ter o campo **PacienteId** e
**MedicoId**. Esses campos são do tipo inteiro e serão passados
manualmente. Não colocar identação (Identity). Quanto à forma de
vincular esses campos às suas tabelas de origem (PacienteId com Codigo
na tabela **Paciente**) será mostrado no próximo passo. Depois de
finalizado o seu diagrama deve conter as 3 tabelas relacionadas.

Integridade Referencial - Chave estrangeira: O fato de você ter criado a
tabela **Consulta** com o campo **PacienteId** e colocar do tipo
inteiro, não é o bastante para evitar, por exemplo, que um usuário
coloque um PacienteId com valor 100 e na tabela **Paciente** esse código
não existir. Já imaginou na hora da consulta informar um código de
paciente que não existe na tabela **Paciente**? Como resolver? Para
resolver isso, vamos informar que o campo **PacienteId** é uma chave
estrangeira do campo **Codigo** na tabela **Paciente**. Passo para ligar
Codigo do Paciente a PacienteId: Clique em cima da chave amarela ao lado
do Codigo na tabela **Paciente**, segure, arraste e solte em cima do
campo PacienteId na tabela **Consulta**.

Na janela que aparecer confirme que na esquerda (chave primária) está a
tabela **Paciente** e o campo Codigo. Na parte da direita (chave
estrangeira) a tabela **Consulta** e o campo PacienteId.

Faça o mesmo processo para vincular o Codigo do Médico na tabela Médico
com o campo MedicoId na tabela **Consulta**.

Atenção! Sempre vá salvando as alterações.

🗄️ Etapa 2B: CRIAÇÃO DO BANCO DE DADOS COM SCRIPT Se você quiser pode
rodar o script a seguir e ele irá criar o banco de dados e as tabelas
com seus campos e relacionamento. E ainda, via insert vai inserir alguns
dados.

SQL

CREATE DATABASE dbClinica;

GO

USE dbClinica;

GO

\-- Tabela Paciente

CREATE TABLE Paciente (

    Codigo INT IDENTITY(1,1) PRIMARY KEY,

    Nome VARCHAR(100) NOT NULL,

    Cpf VARCHAR(14) NOT NULL UNIQUE,

    Telefone VARCHAR(20) NOT NULL,

    DataNascimento DATE NOT NULL

);

GO

\-- Tabela Tecnico

CREATE TABLE Tecnico (

    Codigo INT IDENTITY(1,1) PRIMARY KEY,

    Nome VARCHAR(100) NOT NULL,

    Crm VARCHAR(20) NOT NULL UNIQUE,

    Especialidade VARCHAR(50) NOT NULL

);

GO

\-- Tabela Consulta

CREATE TABLE Consulta (

    Codigo INT IDENTITY(1,1) PRIMARY KEY,

    DataHora DATETIME NOT NULL,

    StatusConsulta VARCHAR(30) NOT NULL,

    PacienteId INT NOT NULL,

    MedicoId INT NOT NULL,

    CONSTRAINT FK_Consulta_Paciente FOREIGN KEY (PacienteId) 

        REFERENCES Paciente(Codigo),

    CONSTRAINT FK_Consulta_Tecnico FOREIGN KEY (MedicoId) 

        REFERENCES Tecnico(Codigo)

);

GO

\-- Inserindo Dados Iniciais para Teste

INSERT INTO Paciente (Nome, Cpf, Telefone, DataNascimento) VALUES 

('Maria Oliveira', '111.222.333-44', '(11) 98888-7777', '1990-05-15'),

('João Souza', '555.666.777-88', '(11) 97777-6666', '1985-10-20');

INSERT INTO Tecnico (Nome, Crm, Especialidade) VALUES 

('Dra. Helena Rios', 'TEC/SP 123456', 'Redes de Computadores'),

('Dr. Roberto Alves', 'TEC/SP 654321', 'Manutenção de Hardware');

INSERT INTO Consulta (DataHora, StatusConsulta, PacienteId, MedicoId)
VALUES 

('2026-10-15 14:00:00', 'Agendada', 1, 1),

('2026-10-16 09:30:00', 'Agendada', 2, 2);

GO

📁 Importação de dados via Arquivo Se você quiser, você pode importar os
dados de 5 pacientes diretamente do arquivo pacientes.csv
disponibilizado. Siga os passos a seguir: Clique com o botão direito do
mouse em seu banco de dados, depois escolha Tarefas e em seguida
Importar Dados. Clique em avançar e na tela seguinte escolha:

-   Fonte de dados: Flat File Source
-   Nome do arquivo: clique em procurar, localize o arquivo
    pacientes.csv (se não exibir mesmo estando na pasta, coloque a opção
    como todos os arquivos ao lado do nome)
-   Clique em próximo
-   Verifique se os dados estão organizados, o delimitador nesse caso é
    vírgula.

Destino:

-   Escolha Microsoft OLE DB Provider for SQL Server
-   Nome do servidor: .`\SENAI`{=tex}
-   Marque a opção: Usar autenticação do SQL Server
-   Usuário: sa ou seu usuário
-   Senha: coloque a senha do seu sql
-   Banco de dados: dbClinica

Ele já vai sugerir a tabela paciente, você pode alterar se desejar, ou
caso não esteja correto. Pode ir avançando e confirmar. Estando tudo
certo se você usar o comando na consulta sql:

SQL

SELECT \* FROM Paciente

Irá retornar os pacientes.

📁 Criação do Projeto no Visual Studio Com nosso banco de dados pronto,
agora é a hora de criarmos o nosso projeto no ASP.NET (Visual Studio).

📦 Etapa 1: Criando o Projeto ASP.NET

1.  Abra o Visual Studio.
2.  Clique em Criar um novo projeto.
3.  Selecione o modelo Web do ASP.NET Core (Model-View-Controller) e
    clique em Próximo.
4.  Defina o nome da solução (ex: **appReversotask**) e clique em
    Próximo.
5.  Selecione o Framework .NET 8.0 (Suporte Médico Padrão) e clique em
    Criar.

📦 Etapa 2: Instalação dos Pacotes do Entity Framework

-   Microsoft.EntityFrameworkCore.SqlServer
-   Microsoft.EntityFrameworkCore.Tools
-   Microsoft.VisualStudio.Web.CodeGeneration.Design

Para instalar você deve usar o Gerenciador de Pacotes NuGet, clicando
com o botão direito do mouse no nome do projeto e clicando em Gerenciar
Pacotes NuGet. A versão do pacote deve ser a mesma da versão .NET de seu
projeto. Se no passo Etapa 01 você escolheu 8.0, na versão do pacote
escolha a maior versão da 8.x.x.

⚙️ Etapa 3: Configuração da String de Conexão no appsettings.json Abra o
arquivo appsettings.json na raiz do projeto e configure a propriedade
ConnectionStrings.

{

  "Logging": {

    "LogLevel": {

      "Default": "Information",

      "Microsoft.AspNetCore": "Warning"

    }

  },

  "AllowedHosts": "\*",

  "ConnectionStrings": {

    "ConexaoSqlServer": "Server=\\`\SENAI`{=tex}; Database=dbClinica;
User Id=sa; Password=senai.123; TrustServerCertificate=True;"

  }

}

Exemplo com Autenticação do Windows (Trusted Connection):

"ConnectionStrings": {

  "ConexaoSqlServer": "Server=\\`\SENAI`{=tex}; Database=dbClinica;
Trusted_Connection=True; TrustServerCertificate=True;"

}

🔨 Etapa 4: Engenharia Reversa - scaffolding update database Você deve
abrir o Console do Gerenciador de Pacotes para executar o comando a
seguir. Ele irá fazer o mapeamento das tabelas e automaticamente irá
criar a classe de contexto, que é responsável pelo mapeamento das
tabelas do banco em classes.

Comando:

PowerShell

Scaffold-DbContext "Name=ConexaoSqlServer"
Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Force

Observe também se dentro da pasta Models foi criada para cada tabela uma
classe. Também, verifique qual o nome que foi dado à classe de Contexto,
provavelmente será DbClinicaContext (ou DbClinicaContext caso mantenha o
prefixo de ambiente). Procure a classe que tenha o sufixo Context, vai
ser ela.

Verifique se alguma classe (paciente, tecnico, consulta) ficou com o
nome diferente, se sim, faça a alteração do nome para igualar ao da
tabela. No caso, se caso o Entity Framework gere um nome diferente para
Consulta, altere o nome do arquivo da classe. Você também deve alterar o
nome dentro da classe DbClinicaContext. Resumo: onde estiver Consulta
alterar para Consulta.

🔨 Etapa 5: Compilação Obrigatória da Solução Antes de gerar as telas e
controllers, o projeto precisa estar limpo e compilado: Pressione Ctrl +
Shift + B ou clique com o botão direito na Solução e selecione
Recompilar (Rebuild).

Etapa 6: Registro do DbContext na Injeção de Dependência (Program.cs)
Para que o gerador de código consiga instanciar o banco sem erros de
tempo de execução ou na geração do Scaffolding, registre o contexto no
contêiner de dependências do .NET. Abra o arquivo Program.cs e insira o
registro antes do var app = builder.Build();

using Microsoft.EntityFrameworkCore;

using appReversotask.Models; // Substitua pelo namespace real das suas
Models

var builder = WebApplication.CreateBuilder(args);

// Adicionar os serviços ao contêiner

builder.Services.AddControllersWithViews();

// Registrando o DbContext com a String de Conexão

builder.Services.AddDbContext`<DbClinicaContext>`{=html}(options =\>

    options.UseSqlServer(builder.Configuration.GetConnectionString("ConexaoSqlServer")));

var app = builder.Build();

🎨 Etapa 7: Gerando o CRUD Automático (Scaffolding MVC) Agora vamos
criar os Controllers e Views Razor sem digitar nenhuma linha de código
manual:

1.  No Gerenciador de Soluções, clique com o botão direito na pasta
    Controllers \> Adicionar \> Item do Scaffolding... (ou New
    Scaffolded Item...).
2.  Selecione a opção Controlador MVC com exibições, usando o Entity
    Framework e clique em Adicionar.
3.  Na janela de configuração:
    -   Classe de modelo: Selecione Paciente (appReversotask.Models).
    -   Classe do contexto de dados: Selecione DbClinicaContext
        (appReversotask.Models).
    -   Exibições: Certifique-se de que a opção de gerar views esteja
        marcada.
4.  Clique em Adicionar.

Repita o mesmo procedimento para a classe de modelo Tecnico e Consulta.
ATENÇÃO: Deixe os nomes da controller no Singular (ex:
PacienteController, MedicoController, ConsultaController).

🎯 Resultado Esperado O Visual Studio gerará automaticamente:

-   PacienteController.cs com as ações de Create, Read, Update, Delete
    (CRUD) prontas.
-   As pastas Views/Paciente com os arquivos .cshtml correspondentes
    (Index, Create, Edit, Details, Delete).

Basta pressionar F5 para executar a aplicação e navegar até /Paciente
para visualizar seu CRUD totalmente funcional conectado ao banco SQL
Server! 🚀

Repita o mesmo processo para Tecnico e Consulta.

🔒 Implementando Autenticação Simples com Session Para nível de
treinamento vamos criar uma estrutura de autenticação login, usando
sessões.

1.  Alterando o ConsultaController: Atualizando o ConsultaController
    para vincular o agendamento diretamente ao paciente autenticado e
    garantir que cada usuário visualize apenas as suas próprias
    consultas.

using System;

using System.Collections.Generic;

using System.Linq;

using System.Threading.Tasks;

using appReversotask.Models;

using Microsoft.AspNetCore.Mvc;

using Microsoft.AspNetCore.Mvc.Rendering;

using Microsoft.EntityFrameworkCore;

namespace appReversotask.Controllers

{

    // Controller responsável pelo gerenciamento de consultas (CRUD)

    public class ConsultaController : Controller

    {

        // Contexto do banco de dados

        private readonly DbClinicaContext \_context;

        // Injeção de dependência do DbContext

        public ConsultaController(DbClinicaContext context)

        {

            \_context = context;

        }

        // GET: Consulta

        // Lista as consultas do paciente autenticado

        public async Task\<IActionResult\> Index()

        {

            // Obtém o ID do paciente logado através da Session

            var pacienteId = HttpContext.Session.GetInt32("PacienteId");

            // Se não houver paciente autenticado, redireciona para o
login

            if (pacienteId == null)

            {

                return RedirectToAction("Login", "Account");

            }

            // Busca somente as consultas pertencentes ao paciente
logado

            var consultas = await \_context.Consulta

                .Include(c =\> c.Medico)

                .Include(c =\> c.Paciente)

                .Where(c =\> c.PacienteId == pacienteId.Value)

                .ToListAsync();

            return View(consultas);

        }

        // GET: Consulta/Details/5

        public async Task\<IActionResult\> Details(int? id)

        {

            if (id == null)

            {

                return NotFound();

            }

            var consulta = await \_context.Consulta

                .Include(c =\> c.Medico)

                .Include(c =\> c.Paciente)

                .FirstOrDefaultAsync(m =\> m.Codigo == id);

            if (consulta == null)

            {

                return NotFound();

            }

            return View(consulta);

        }

        // GET: Consulta/Create

        public IActionResult Create()

        {

            // Lista de médicos disponíveis

            ViewData\["MedicoId"\] = new SelectList(

                \_context.Medicos,

                "Codigo",

                "Nome"

            );

            return View();

        }

        // POST: Consulta/Create

        \[HttpPost\]

        \[ValidateAntiForgeryToken\]

        public async Task\<IActionResult\> Create(

            \[Bind("DataHora,StatusConsulta,MedicoId")\] Consulta
consulta)

        {

            // Obtém o ID do paciente autenticado

            var pacienteId = HttpContext.Session.GetInt32("PacienteId");

            // Caso não esteja autenticado, retorna para o login

            if (pacienteId == null)

            {

                return RedirectToAction("Login", "Account");

            }

            // Vincula automaticamente a consulta ao paciente logado

            consulta.PacienteId = pacienteId.Value;

            if (ModelState.IsValid)

            {

                \_context.Add(consulta);

                await \_context.SaveChangesAsync();

                return RedirectToAction(nameof(Index));

            }

            // Recarrega a lista de médicos caso exista erro de
validação

            ViewData\["MedicoId"\] = new SelectList(

                \_context.Medicos,

                "Codigo",

                "Nome",

                consulta.MedicoId

            );

            return View(consulta);

        }

        // GET: Consulta/Edit/5

        public async Task\<IActionResult\> Edit(int? id)

        {

            if (id == null)

            {

                return NotFound();

            }

            var consulta = await \_context.Consulta.FindAsync(id);

            if (consulta == null)

            {

                return NotFound();

            }

            ViewData\["MedicoId"\] = new SelectList(

                \_context.Medicos,

                "Codigo",

                "Nome",

                consulta.MedicoId

            );

            ViewData\["PacienteId"\] = new SelectList(

                \_context.Pacientes,

                "Codigo",

                "Nome",

                consulta.PacienteId

            );

            return View(consulta);

        }

        // POST: Consulta/Edit/5

        \[HttpPost\]

        \[ValidateAntiForgeryToken\]

        public async Task\<IActionResult\> Edit(

            int id,

            \[Bind("Codigo,DataHora,StatusConsulta,PacienteId,MedicoId")\]
Consulta consulta)

        {

            if (id != consulta.Codigo)

            {

                return NotFound();

            }

            if (ModelState.IsValid)

            {

                try

                {

                    \_context.Update(consulta);

                    await \_context.SaveChangesAsync();

                }

                catch (DbUpdateConcurrencyException)

                {

                    if (!ConsultaExists(consulta.Codigo))

                    {

                        return NotFound();

                    }

                    else

                    {

                        throw;

                    }

                }

                return RedirectToAction(nameof(Index));

            }

            ViewData\["MedicoId"\] = new SelectList(

                \_context.Medicos,

                "Codigo",

                "Nome",

                consulta.MedicoId

            );

            ViewData\["PacienteId"\] = new SelectList(

                \_context.Pacientes,

                "Codigo",

                "Nome",

                consulta.PacienteId

            );

            return View(consulta);

        }

        // GET: Consulta/Delete/5

        public async Task\<IActionResult\> Delete(int? id)

        {

            if (id == null)

            {

                return NotFound();

            }

            var consulta = await \_context.Consulta

                .Include(c =\> c.Medico)

                .Include(c =\> c.Paciente)

                .FirstOrDefaultAsync(m =\> m.Codigo == id);

            if (consulta == null)

            {

                return NotFound();

            }

            return View(consulta);

        }

        // POST: Consulta/Delete/5

        \[HttpPost, ActionName("Delete")\]

        \[ValidateAntiForgeryToken\]

        public async Task\<IActionResult\> DeleteConfirmed(int id)

        {

            var consulta = await \_context.Consulta.FindAsync(id);

            if (consulta != null)

            {

                \_context.Consulta.Remove(consulta);

            }

            await \_context.SaveChangesAsync();

            return RedirectToAction(nameof(Index));

        }

        // Verifica se a consulta existe

        private bool ConsultaExists(int id)

        {

            return \_context.Consulta.Any(e =\> e.Codigo == id);

        }

    }

}

2.  Alterando a view de CREATE CHAMADO:

@\* Define o tipo de modelo fortemente tipado esperado por esta View \*@

@model appReversotask.Models.Consulta

\@{

    // Define o título da página no layout compartilhado
(\_Layout.cshtml)

    ViewData\["Title"\] = "Criar Consulta";

}

\<h1\>Create\</h1\>

\<h4\>Consulta\</h4\>

\<hr /\>

\<div class="row"\>

    \<div class="col-md-4"\>

        @\* Formulário configurado via Tag Helper para enviar os dados
via POST para a Action 'Create' do ConsultaController \*@

        \<form asp-action="Create"\>

            @\* Exibe mensagens de erro globais referentes ao Model \*@

            \<div asp-validation-summary="ModelOnly"
class="text-danger"\>\</div\>

            @\* Campo para entrada da Data e Hora da consulta \*@

            \<div class="form-group mb-3"\>

                @\* Rótulo gerado automaticamente com base na
propriedade DataHora \*@

                \<label asp-for="DataHora"
class="control-label"\>\</label\>

                @\* Input vinculado à propriedade DataHora \*@

                \<input asp-for="DataHora" class="form-control" /\>

                @\* Exibe a mensagem de validação específica do campo
DataHora \*@

                \<span asp-validation-for="DataHora"
class="text-danger"\>\</span\>

            \</div\>

            @\* Campo de seleção para o Status da consulta \*@

            \<div class="form-group mb-3"\>

                \<label asp-for="StatusConsulta"
class="control-label"\>\</label\>

                @\* 

                    Dropdown com os possíveis estados da consulta.

                    O valor selecionado será enviado para a propriedade
StatusConsulta.

                \*@

                \<select asp-for="StatusConsulta" class="form-control"\>

                    \<option value=""\>-- Selecione o Status
--\</option\>

                    \<option value="Agendada"\>Agendada\</option\>

                    \<option value="Realizada"\>Realizada\</option\>

                    \<option value="Cancelada"\>Cancelada\</option\>

                \</select\>

                @\* Exibe a mensagem de validação específica do campo
StatusConsulta \*@

                \<span asp-validation-for="StatusConsulta"
class="text-danger"\>\</span\>

            \</div\>

            @\* Campo de seleção para o Médico responsável pela consulta
\*@

            \<div class="form-group mb-3"\>

                @\* Define o texto exibido no rótulo como 'Médico' \*@

                \<label asp-for="MedicoId"
class="control-label"\>Médico\</label\>

                @\* 

                    Popula o dropdown utilizando a SelectList

                    enviada pela ConsultaController através da ViewBag.

                \*@

                \<select asp-for="MedicoId"

                        class="form-control"

                        asp-items="ViewBag.MedicoId"\>

                    \<option value=""\>-- Selecione o Médico
--\</option\>

                \</select\>

                @\* Exibe a mensagem de validação específica do campo
MedicoId \*@

                \<span asp-validation-for="MedicoId"
class="text-danger"\>\</span\>

            \</div\>

            @\*

                O PacienteId NÃO aparece no formulário.

                O paciente é definido automaticamente pela
ConsultaController

                através da sessão do usuário autenticado.

            \*@

            @\* Botões de ação do formulário \*@

            \<div class="form-group mt-3"\>

                @\* Botão responsável por enviar o formulário para a
Action Create \*@

                \<input type="submit"

                       value="Cadastrar"

                       class="btn btn-primary" /\>

                @\* Link para retornar à listagem de consultas \*@

                \<a asp-action="Index"

                   class="btn btn-secondary"\>

                    Voltar

                \</a\>

            \</div\>

        \</form\>

    \</div\>

\</div\>

@section Scripts {

    @\* Carrega os scripts de validação do ASP.NET Core \*@

    @{

        await Html.RenderPartialAsync("\_ValidationScriptsPartial");

    }

}

3.  Criando a View Model de Login: Dentro da pasta Models, crie uma nova
    classe chamada LoginViewModel.cs. Ela servirá para capturar os dados
    do formulário de login.

// Importa o namespace responsável pelas validações de dados

// e formatação visual através de Data Annotations.

using System.ComponentModel.DataAnnotations;

namespace appReversotask.Models

{

    // ViewModel utilizada exclusivamente para capturar

    // e validar os dados de entrada na tela de Login.

    public class LoginViewModel

    {

        // Define que o CPF é obrigatório.

        // Caso o usuário não informe o CPF, será exibida

        // a mensagem personalizada.

        \[Required(ErrorMessage = "O CPF é obrigatório.")\]

        // Define o nome amigável que será exibido

        // nas Views Razor através do \<label asp-for="Cpf"\>.

        \[Display(Name = "CPF do Paciente")\]

        // Armazena o CPF informado pelo paciente.

        // Inicializado como string vazia para evitar

        // avisos relacionados à nulidade.

        public string Cpf { get; set; } = string.Empty;

    }

}

4.  Criar Controller de Login: Clique com o botão direito na pasta
    Controllers \> Adicionar \> Controlador... \> Escolha Controlador
    MVC - Vazio e nomeie como AccountController.cs. Adicione o código
    abaixo para gerenciar o Login e o Logout do paciente:

using appReversotask.Models;

using Microsoft.AspNetCore.Authentication;

using Microsoft.AspNetCore.Authentication.Cookies;

using Microsoft.AspNetCore.Mvc;

using Microsoft.EntityFrameworkCore;

using System.Security.Claims;

namespace appReversotask.Controllers

{

    public class AccountController : Controller

    {

        private readonly DbClinicaContext \_context;

        public AccountController(DbClinicaContext context)

        {

            \_context = context;

        }

        // GET: /Account/Login

        \[HttpGet\]

        public IActionResult Login()

        {

            // Verifica se o paciente já está autenticado

            if (User.Identity != null && User.Identity.IsAuthenticated)

            {

                return RedirectToAction("Index", "Consulta");

            }

            return View();

        }

        // POST: /Account/Login

        \[HttpPost\]

        \[ValidateAntiForgeryToken\]

        public async Task\<IActionResult\> Login(LoginViewModel model)

        {

            // Verifica se os dados informados são válidos

            if (!ModelState.IsValid)

            {

                return View(model);

            }

            // Busca o paciente pelo CPF informado

            var paciente = await \_context.Pacientes

                .FirstOrDefaultAsync(p =\> p.Cpf == model.Cpf);

            // Caso o CPF não seja encontrado

            if (paciente == null)

            {

                ModelState.AddModelError(

                    "",

                    "CPF não encontrado. Faça seu cadastro primeiro."

                );

                return View(model);

            }

            // Cria os dados de autenticação do paciente

            var claims = new List\<Claim\>

            {

                // Identifica o paciente através do Código

                new Claim(

                    ClaimTypes.NameIdentifier,

                    paciente.Codigo.ToString()

                ),

                // Nome do paciente

                new Claim(

                    ClaimTypes.Name,

                    paciente.Nome

                ),

                // CPF do paciente

                new Claim(

                    "CPF",

                    paciente.Cpf

                )

            };

            // Cria a identidade utilizando autenticação por Cookies

            var claimsIdentity = new ClaimsIdentity(

                claims,

                CookieAuthenticationDefaults.AuthenticationScheme

            );

            // Realiza o login do paciente

            await HttpContext.SignInAsync(

                CookieAuthenticationDefaults.AuthenticationScheme,

                new ClaimsPrincipal(claimsIdentity)

            );

            // Salva o ID do paciente na Session

            // para ser utilizado posteriormente pela ConsultaController

            HttpContext.Session.SetInt32(

                "PacienteId",

                paciente.Codigo

            );

            // Após o login, direciona o paciente para a lista de
consultas

            return RedirectToAction("Index", "Consulta");

        }

        // GET: /Account/Logout

        public async Task\<IActionResult\> Logout()

        {

            // Limpa todos os dados armazenados na Session

            HttpContext.Session.Clear();

            // Encerra a autenticação por Cookies

            await HttpContext.SignOutAsync(

                CookieAuthenticationDefaults.AuthenticationScheme

            );

            // Retorna para a tela de login

            return RedirectToAction("Login");

        }

    }

}

A proposta aqui é que toda vez que o Login for válido pelo CPF, ele
adiciona uma sessão no navegador e salva o código e nome do paciente.

5.  Criar a View Login: Dentro da pasta Views, crie uma nova pasta
    chamada Account. Dentro de Views/Account, crie um arquivo Razor View
    consulta Login.cshtml com o seguinte conteúdo:

@\* Define o modelo utilizado pela tela de Login \*@

@model appReversotask.Models.LoginViewModel

\@{

    // Define o título da página

    ViewData\["Title"\] = "Acesso ao Sistema";

}

\<div class="row justify-content-center mt-5"\>

    \<div class="col-md-4"\>

        \<div class="card shadow-sm"\>

            @\* Cabeçalho do cartão de Login \*@

            \<div class="card-header bg-primary text-white
text-center"\>

                \<h4\>Clínica\</h4\>

                \<small\>Acesso do Paciente\</small\>

            \</div\>

            \<div class="card-body"\>

                @\* 

                    Formulário de Login.

                    Os dados serão enviados via POST para a Action Login

                    do AccountController.

                \*@

                \<form asp-action="Login" method="post"\>

                    @\* Exibe erros gerais de validação \*@

                    \<div asp-validation-summary="ModelOnly"

                         class="text-danger mb-3"\>

                    \</div\>

                    @\* Campo para informar o CPF \*@

                    \<div class="form-group mb-3"\>

                        @\* Label definido através do atributo Display
do LoginViewModel \*@

                        \<label asp-for="Cpf"

                               class="form-label"\>

                        \</label\>

                        @\* Campo de entrada do CPF \*@

                        \<input asp-for="Cpf"

                               class="form-control"

                               placeholder="Digite seu CPF (ex:
111.222.333-44)" /\>

                        @\* Mensagem de validação do CPF \*@

                        \<span asp-validation-for="Cpf"

                              class="text-danger"\>

                        \</span\>

                    \</div\>

                    @\* Botão de Login \*@

                    \<div class="d-grid gap-2"\>

                        \<button type="submit"

                                class="btn btn-primary"\>

                            Entrar

                        \</button\>

                    \</div\>

                \</form\>

            \</div\>

            @\* Rodapé com opção de cadastro \*@

            \<div class="card-footer text-center"\>

                \<p class="mb-0"\>

                    Ainda não tem cadastro?

                \</p\>

                @\* 

                    Direciona o paciente para o cadastro.

                    O Controller utilizado agora é Paciente.

                \*@

                \<a asp-controller="Paciente"

                   asp-action="Create"

                   class="btn btn-link"\>

                    Cadastre-se aqui

                \</a\>

            \</div\>

        \</div\>

    \</div\>

\</div\>

@section Scripts {

    @\* Carrega os scripts de validação do ASP.NET Core \*@

    @{

        await Html.RenderPartialAsync("\_ValidationScriptsPartial");

    }

}

6.  No arquivo Program.cs, insira a configuração de Session e a
    Autenticação por Cookie na área de registro de serviços. Em seguida,
    habilite a ordem correta dos middlewares na pipeline. Abra o arquivo
    Program.cs e ative o serviço de autenticação por cookies e sessão.
    Insira a configuração antes de var app = builder.Build();:

// Adicionar autenticação por Cookie e suporte a Session

builder.Services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)

    .AddCookie(options =\>

    {

        options.LoginPath = "/Account/Login";

        options.AccessDeniedPath = "/Account/Login";

    });

builder.Services.AddSession(options =\>

{

    options.IdleTimeout = TimeSpan.FromMinutes(30);

    options.Cookie.HttpOnly = true;

    options.Cookie.IsEssential = true;

});

Na pipeline de middlewares (após var app = builder.Build();):

app.UseStaticFiles();

app.UseRouting();

// É fundamental registrar a Session e a Autenticação nesta ordem exata
antes de UseAuthorization

app.UseSession();

app.UseAuthentication();

app.UseAuthorization();

🔒 Etapa 7: Protegendo as Rotas com o atributo **\[Authorize\]**

Agora que a infraestrutura de login está pronta, precisamos proteger o
ConsultaController para proibir o acesso de pessoas não logadas.

Abra o arquivo Controllers/ConsultaController.cs e adicione o atributo
\[Authorize\] acima da classe:

using Microsoft.AspNetCore.Authorization;

namespace appReversotask.Controllers

{

    \[Authorize\] // Impede o acesso de usuários não autenticados

    public class ConsultaController : Controller

    {

        // Métodos do controller mantidos...

    }

}

🖼️ Etapa 8: Atualizando o Layout da Aplicação (\_Layout.cshtml)

Para que o paciente saiba quem está logado, consiga navegar para a
abertura de consultas e tenha a opção de fazer **Logout**, abra o
arquivo Views/Shared/\_Layout.cshtml e insira o trecho abaixo na barra
de navegação (\<ul class="navbar-nav ..."\>):

\<ul class="navbar-nav ms-auto"\>

    @if (User.Identity != null && User.Identity.IsAuthenticated)

    {

        \<li class="nav-item"\>

            \<a class="nav-link text-dark"

               asp-controller="Consulta"

               asp-action="Index"\>

                Minhas Consultas

            \</a\>

        \</li\>

        \<li class="nav-item"\>

            \<span class="nav-link text-dark"\>

                Olá, \<strong\>@User.Identity.Name\</strong\>

            \</span\>

        \</li\>

        \<li class="nav-item"\>

            \<a class="nav-link text-danger"

               asp-controller="Account"

               asp-action="Logout"\>

                Sair

            \</a\>

        \</li\>

    }

    else

    {

        \<li class="nav-item"\>

            \<a class="nav-link text-primary"

               asp-controller="Account"

               asp-action="Login"\>

                Entrar

            \</a\>

        \</li\>

    }

\</ul\>

 Etapa 9: Testando o Fluxo Completo, altere o Index do
**ConsultaController **

Insira o filtro:

public async Task\<IActionResult\> Index(string pesquisa)

{

    // Obtém o ID do paciente logado

    var pacienteId = HttpContext.Session.GetInt32("PacienteId");

    // Se não estiver autenticado, volta para o Login

    if (pacienteId == null)

    {

        return RedirectToAction("Login", "Account");

    }

    // Busca somente as consultas do paciente logado

    var consultas = \_context.Consulta

        .Include(c =\> c.Medico)

        .Include(c =\> c.Paciente)

        .Where(c =\> c.PacienteId == pacienteId.Value);

    // Aplica o filtro caso o usuário tenha digitado alguma coisa

    if (!string.IsNullOrWhiteSpace(pesquisa))

    {

        pesquisa = pesquisa.Trim();

        consultas = consultas.Where(c =\>

            c.StatusConsulta.Contains(pesquisa) \|\|

            c.Medico.Nome.Contains(pesquisa) \|\|

            c.DataHora.ToString().Contains(pesquisa)

        );

    }

    return View(await consultas.ToListAsync());

}

Agora coloque o campo de pesquisa no Views/Consulta/Index.cshtml

Coloque antes da tabela:

\<div class="row mb-4"\>

    \<div class="col-md-8"\>

        \<form asp-action="Index" method="get"\>

            \<div class="input-group"\>

                \<input type="text"

                       name="pesquisa"

                       class="form-control"

                       placeholder="Pesquisar por médico, status ou
data..." /\>

                \<button type="submit" class="btn btn-primary"\>

                    🔎 Pesquisar

                \</button\>

                \<a asp-action="Index" class="btn btn-secondary"\>

                    Limpar

                \</a\>

            \</div\>

        \</form\>

    \</div\>

\</div\>

 Etapa 10: Testando o Fluxo Completo

1.  Execute a aplicação (**F5**).
2.  A primeira tela deverá ser a tela de Login (`/Account/Login`).
3.  Tente acessar diretamente a rota `/Consulta` sem estar autenticado.
    O atributo `[Authorize]` deverá redirecionar para o Login.
4.  Digite o CPF de teste cadastrado no script SQL: `111.222.333-44` e
    clique em **Entrar**.
5.  Após o login, o sistema deverá redirecionar para a listagem de
    **Minhas Consultas**.
6.  Clique em **Nova Consulta**.
7.  Informe a data e hora, selecione um médico e informe o status da
    consulta.
8.  O paciente não deverá selecionar seu próprio cadastro. O
    `PacienteId` será obtido automaticamente da Session.
9.  Após salvar, a consulta deverá aparecer na listagem de **Minhas
    Consultas**.
10. Faça uma pesquisa por médico, status ou data utilizando o campo de
    pesquisa.
11. Clique em **Limpar** para retornar à listagem completa das consultas
    do paciente.
12. Teste os recursos de **Detalhes**, **Editar** e **Excluir**.
13. Se digitar um CPF inexistente, o sistema deverá informar que o CPF
    não foi encontrado e disponibilizar a opção para realizar o cadastro
    do paciente.
14. Faça logout e tente acessar novamente `/Consulta`. O sistema deverá
    exigir novo login.

### 🎯 Resultado esperado

Ao final do teste, o fluxo principal deverá ser:

**Login → Minhas Consultas → Nova Consulta → Selecionar Médico →
Informar Data/Hora → Salvar → Consultar/Editar/Excluir**

O paciente autenticado deverá visualizar somente as consultas vinculadas
ao seu próprio `PacienteId`.

