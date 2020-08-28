

- ## Comandos : Criando um ASP.NET Core project
  - 'cd ..' para sair da pasta do CsharpHelloWorld;
  - 'mkdir AspNetCoreTodo', cria uma nova pasta onde após o mkdir é o nome e pode-se criar com outro nome de sua preferência;
  - 'cd AspNetCoreTodo' Entra na respectiva pasta, lembrando que, se o nome da mesma foi modificado, deve-se escrever o nome escolhido após o comando cd;
  - 'dotnet new mvc --auth Individual -o AspNetCoreTodo', para criar um projeto com comando MVC;
  - 'cd AspNetCoreTodo' para entrar na nova pasta criada;
  - 'dotnet run' executa o programa e deve retornar:
     
      **info: Microsoft.Hosting.Lifetime[0]<br/>**
      **Now listening on: https://localhost:5001<br/>**
      **info: Microsoft.Hosting.Lifetime[0]<br/>**
      **Now listening on: http://localhost:5000<br/>**
      **info: Microsoft.Hosting.Lifetime[0]<br/>**
      **Application started. Press Ctrl+C to shut down.<br/>**
      **info: Microsoft.Hosting.Lifetime[0]<br/>**
      **Hosting environment: Development<br/>**
      **info: Microsoft.Hosting.Lifetime[0]<br/>**
      **Content root path: C:\Users\xxxx\Desktop\littleAspNet\AspNetCoreTodo\AspNetCoreTodo<br/>**
   
   e após isso abra o navegado em http://localhost:5000, e ele deve aparecer a pagina com " Welcome Learn about building Web apps with ASP.NET Core." , para parar o serviço 'Ctrl + C';
 - Conteúdo : ASP.NET Core project
   - **Program.cs** e **Startup.cs** são classes que configuram o servidor web e ASP.NET Core pipeline;
   - **Models, Views**, e **Controllers** são diretórios contêm os componentes da arquitetura Model-View-Controller (MVC);
   - **wwwroot** contém ativos estáticos que podem ser agrupados e compactados automaticamente, como CSS, JavaScript e arquivos de imagem;
   - **appsettings.json** contém as configuração de inicialização que o ASP.NETCore carrega; 

- ## Basico de MVC :

  **Por padrão o funcionamento dos elementos ocorre da seguinte forma:**<br />
  - :video_game: O **Controller**:
    - Recebe uma solicitação e consulta algumas informações no banco de dados,
    - Cria um modelo com as informações e o anexa a uma view;
    - A **View** é renderizada e exibida no navegador do usuário;
    - Então o usuário clica em um botão ou envia um formulário, que envia uma nova solicitação ao controlador e o ciclo se repete;

- ## Continuando o projeto ASP.NET Core : Controller
  **Agora que sabemos o que é um controller vamos contruir um:**<br />
  - Se abrirmos a pasta Controllers, veremos que ja existe um HomeController.cs que inclui três métodos de ação (Index, About, e Contact) que são mapeados pelo ASP.NET Core para esses URLs de rota;
  - Pelo VS Code clicando na pasta Controllers, você pode criar um "new file" chamado TodoController não se esqueça da exentensão .cs;
  - E escreva o seguinte código:
  
 ```
 using System;
 using System.Collections.Generic;
 using System.Linq;using System.Threading.Tasks;
 using Microsoft.AspNetCore.Mvc;

  namespace AspNetCoreTodo.Controllers
  {
    public class TodoController : Controller    
    {
    // Actions go here
     }
  }
  ```
   - Substitua "//Actions " go here por 
  ```
  public IActionResult Index()
    {// Get to-do items from database
    // Put items into a model
    // Render view using the model    }
     }
  ```
  - O objetivo do método IActionResult é retornar códigos de status HTTP como 200 (OK) e 404 (não encontrado), views, ou dados JSON.

- :dancer: **O Model**:
  - Vamos criar um modelo que represente um item de tarefa pendente armazenado no banco de dados (às vezes chamado de entidade) e o modelo que será combinado com uma visualização (o MV no MVC) e enviado de volta ao navegador do usuário. 
  - Primeiro criamos uma Classe chamada TodoItem.cs, dentro da pasta Models;
  - Ela define o que o banco de dados precisará armazenar para cada item de tarefa: 
    - Um ID : um guid ou um identificador globalmente exclusivos e são gerados aleatoriamente, para que você não precise se preocupar com o incremento automático
    - Um título ou nome : valor do texto). Isso conterá a descrição do nome do item de pendências. O atributo [Required] informa que o campo é obrigatório.
    - IsDone : valor booleano. 
    - DueAt: informa se o item está completo e qual é a data do vencimento.
    - Agora não importa qual seja a tecnologia de banco de dados implícito. Pode ser SQL Server, MySQL, MongoDB, Redis ou algo mais exótico. Esse modelo define como será a linha ou entrada do banco de dados em C #, para que você não precise se preocupar com as coisas de baixo nível do banco de dados em seu código. Esse estilo simples de modelo às vezes é chamado de "objeto C # antigo simples" ou POCO.
  - E escreva o seguinte código:
  
  ```
  using System;
  using System.ComponentModel.DataAnnotations;
  namespace AspNetCoreTodo.Models 
  {
    public class TodoItem    
    {
        public Guid Id { get; set; }
        public bool IsDone { get; set; }
        [Required]
        public string Title { get; set; }
        public DateTimeOffset? DueAt { get; set; }   
    }
  }
  ```
 
- :sunglasses: **A View model:**  
  - Geralmente o usuario costuma procastinar então o modelo (entidade), que não exatamente o mesmo que o modelo que você deseja usar no MVC (o modelo de exibição), mas a exibição pode ser necessário exibir dois, dez ou cem itens de tarefas pendentes, Por esse motivo, o modelo de exibição deve ser uma classe separada que contém uma matriz de TodoItem;
  - Crie uma classe em Models chamada TodoViewModel.cs
  - E escreva o seguinte código:
 ```  
  namespace AspNetCoreTodo.Models 
{
    public class TodoViewModel    {
        public TodoItem[] Items { get; set;}    
  }
}
```
-:eyeglasses: **A View:** 
  - Uma Views no ASP.NET Core são criados usando a linguagem de modelagem Razor, que combina código HTML e C#.
  - No começo da classe vemos,"@model" que diz diz ao Razor qual modelo esperar que a view está vinculada.
  - Se houver itens de pendências no Model.Items, a declaração de cada loop fará um loop sobre cada item de pendência e renderizará uma linha da tabela (elemento <tr>) contendo o nome e a data de vencimento do item. Uma caixa de seleção está desativada, permitindo que o usuário marque o item como completo.
  - Crie uma pasta "Todo" dentro do diretório Views;
  - E dentro da pasta Todo crie um arquivo "Index.cshtml"
  - E escreva o seguinte código:
  
  ```  
  @model TodoViewModel

  @{    
    ViewData["Title"] = "Manage your todo list";
  }
  <divclass="panel panel-default todo-panel">
    <divclass="panel-heading">@ViewData["Title"]</div>
    
  <tableclass="table table-hover">
    <thead>
        <tr>
            <td>&#x2714;</td>
            <td>Item</td>
            <td>Due</td>
        </tr>
    </thead>      
    
    @foreach (var item in Model.Items)      {
        <tr>
            <td>
                <inputtype="checkbox"class="done-checkbox">
                </td>
                <td>@item.Title</td>
                <td>@item.DueAt</td>
        </tr>      
    }
  </table>

  <divclass="panel-footer add-item-form">
  <!-- TODO: Add item form -->
  </div>
  </div>
  ```
  
-  :book: ## Layout:
   - Sobre o o restante do HTML, está na pasta Views/Shared/_Layout.cshtml, com templates Bootstrap e jQuery;
   - Também contem algumas configurações simples de CSS
   - O stylesheet está na pasta wwwroot/css
   - E escreva o seguinte código para adicionar algumas novas features no final do código do arquivo site.css :
   
   ```
    div.todo-panel {
   margin-top: 15px;
   }
   tabletr.done {
    text-decoration: line-through;
   color: #888;
   }
   ```
  
- :construction_worker: ## Criando uma classe de serviço:
  - Pode-se fazê-la diretamente no Controller porém por boas praticas e no mundo real o ideal é que o código seja separado, pois as classes seram muito maiores, deixando dificil a manipulação podento ter as seguintes preocupações:
    - **Renderização de views** e manipulção de dados recebidos: é isso que o seu controlador já faz.
    - **Executar lógica business**, ou código e lógica relacionados ao objetivo e "negócios" da sua aplicação. Por exemplo: lógica de negócios incluem o cálculo de um custo total com base nos preços e taxas de produtos ou verificar se um jogador tem pontos suficientes para subir de nível em um jogo. 
    - **Manipulação de um banco de dados**.
  - O ideal de um projeto organizado é mante-lo nas arquiteturas multi-tier ou n-tier;
  - Neste projeto, você usaremos duas camadas de aplicativos:
    - Uma camada de apresentação(**presentation layer**) composta pelos controladores e viwes que interagem com o usuário 
    - E uma camada de serviço(**service layer**) que contém lógica de negócios e código do banco de dados. 
    - como a camada de apresentação já existe vamos criar um serviço que lide com a lógica de negócios de tarefas pendentes e salva itens de tarefas pendentes em um banco de dados.
 - Criando a interface:
   - Em C# tem a concepção de **Interface**, onde as interfaces facilitam manter suas classes separadas e fáceis de testar.
   - Então por convenção, as interfaces são prefixadas com "I".
   - Crie um novo diretório chamado Services e dentro dele um arquivo  chamado ITodoItemService.cs :
   - Escreva o seguinte código: 
  
  ```
  using System;
  using System.Collections.Generic;
  using System.Threading.Tasks;
  using AspNetCoreTodo.Models;

  namespace AspNetCoreTodo.Services
  {
      public interface ITodoItemService   
      {        
          Task<TodoItem[]> GetIncompleteItemsAsync();   
      }
  }
 
  ```
  - Veja que o namespace desse arquivo é AspNetCoreTodo.Services. Em .NET e é comum que o namespace siga o diretório em que o arquivo está armazenado, pois Namespaces são uma maneira de organizar arquivos de código.
  - Com interface está definida, será criada a classe de serviço atual.
  - Na pasta Services crie um arquivo chamado "FakeTodoItemService.cs" e escreva o seginte código:
   ```
  using System;
  using System.Collections.Generic;
  using System.Threading.Tasks;
  using AspNetCoreTodo.Models;

  namespace AspNetCoreTodo.Services
  {
      public class FakeTodoItemService : ITodoItemService    
      {
         public Task<TodoItem[]> GetIncompleteItemsAsync()       
          {
               var item1 = new TodoItem
                  {
                    Title = "Learn ASP.NET Core", 
                    DueAt = DateTimeOffset.Now.AddDays(1)  
                  };
               var item2 = new TodoItem
                 {
                      Title = "Build awesome apps",
                     DueAt = DateTimeOffset.Now.AddDays(2)
                  };
                  return Task.FromResult(new[] { item1, item2 }); 
        }   
      }
    }
   ```
   - Para testar  o controlador e a visualização e, em seguida, adicionar o código de banco de dados real, podemos ver que FakeTodoItemService implementa a interface ITodoItemService, mas sempre retorna o mesmo array de dois TodoItems. 
   
- :syringe: ## Usando injeção de dependência:
  - É utilizada quando uma solicitação chega e é roteada para o TodoController, o ASP.NET Core examina os serviços disponíveis e fornece automaticamente o FakeTodoItemService quando o controlador solicita umITodoItemService. Como os serviços são "injetados" no contêiner de serviços, esse padrão é chamado de injeção de dependência;
  - Vamos voltar em TodoController, tarabalhar com o ITodoItemService e escreva o seginte código:
     
   ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using AspNetCoreTodo.Services;
    using Microsoft.AspNetCore.Mvc;

    namespace AspNetCoreTodo.Controllers
    {
      public class TodoController : Controller    
      {
        private readonly ITodoItemService _todoItemService;
        public TodoController(ITodoItemService todoItemService) 
        {
                _todoItemService = todoItemService;    
        }
        public IActionResult Index()
         {
        
         }
      }
   }
   ```
   - Variavél ITodoItemServic, deixa usar o serviço do metodo Index;
   - A linha public TodoController(ITodoItemService todoItemService), define o construtor da classe;
   - Para configurar os serviços vá a classe chamada Startup.cs e modifique:
   ```
   using AspNetCoreTodo.Services;
   public void ConfigureServices(IServiceCollection services)
   {
      services.AddMvc();
      services.AddSingleton<ITodoItemService, FakeTodoItemService>();
   }        
   ```
   - O método ConfigureServices adiciona coisas ao servicecontainer ou à coleção de serviços que o ASP.NET Core conhece;
   - A linha services.AddMvc adiciona os serviços internos do ASP.NETCore. Qualquer outro serviço que você deseja usar em seu aplicativo deve ser adicionado ao contêiner de serviço aqui em ConfigureServices.
   - A linha services.AddSingleton<ITodoItemService, FakeTodoItemService>(); informa ao ASP.NET  para usar o FakeTodoItemService quando a interface ITodoItemService é solicitada em um construtor (ou em qualquer outro lugar);

 - ## Terminando o Controller:
    - A última etapa é terminar o código do controlador. O controlador agora tem uma lista de itens de tarefas pendentes da camada de serviço e precisa colocar esses itens em um TodoViewModel e vincular esse modelo à visualização que você criou anteriormente:
    ```
    using AspNetCoreTodo.Services;
    using AspNetCoreTodo.Models;
    
    public async Task<IActionResult> Index()
      {
        var items = await _todoItemService.GetIncompleteItemsAsync();

        var model = new TodoViewModel()    
       {       
             Items = items    
        };
       return View(model);
    ```
  - ## Testando:
      - Agora para testar o projeto abra um terminal no seu VSCode e digite 'dotnet run';
      - Ele deve retrornar da seguinte forma no terminal:
   
    **info: Microsoft.Hosting.Lifetime[0]<br/>**
      **Now listening on: https://localhost:5001<br/>**
    **info: Microsoft.Hosting.Lifetime[0]<br/>**
     **Now listening on: http://localhost:5000<br/>**
    **info: Microsoft.Hosting.Lifetime[0]<br/>**
      **Application started. Press Ctrl+C to shut down.<br/>**
    **info: Microsoft.Hosting.Lifetime[0]<br/>**
      **Hosting environment: Development<br/>**
    **info: Microsoft.Hosting.Lifetime[0]<br/>**
      **Content root path: C:\Users\User\Desktop\LittleAspnet\AspNetCoreTodo\AspNetCoreTodo<br/>**
     
     - Na pagina http://localhost:5000/ vai aparecer a seguinte mensagem  My to-dos na barra de navegação. Para fazer isso, você pode editar o arquivo de layout compartilhado.
  
  - ## Atualizando o Layout:
     - No arquivo de layout Views/Shared/_Layout.cshtml contém o HTML "base" para cada view. Dessa Forma podemos colocar nosvos elementos aos layout substituindo o seguinte código por:
      ``` 
       <ul class="navbar-nav flex-grow-1">
           <li class="nav-item"><a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a></li>
           <li class="nav-item"><a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a></li>
       </ul>
      ``` 
     - Substituir por 
     ```
     <ul class="nav navbar-nav">
         <li><a asp-area="" asp-controller="Home" asp-action="Index">Home</a></li>
         <li><a asp-area="" asp-controller="Home" asp-action="About">About</a></li>
         <li><a asp-area="" asp-controller="Home" asp-action="Contact">Contact</a></li>
          <li><a asp-controller="Todo"asp-action="Index">My to-dos</a></li>
     </ul>
     ```
  - ## Adicinar pacotes externos
    - Vamos utilizar o Nuget, que possui link em Dowloads;
    -Existem pacotes disponíveis no NuGet para tudo, desde analisar XML para aprendizado de máquina até postar no Twitter. O ASP.NET Core propriamente dito nada mais é do que uma coleção de pacotes NuGet que são adicionados ao seu projeto;
    - **Instalação:**
      - Na documentação vamos no link: https://docs.microsoft.com/en-us/nuget/install-nuget-client-tools e baixe o nuget.exe
      - Coloque-o numa pasta adequada, ex.C: ;
      - E por fim adicione à variavel de ambientes PATH.
      - Após isso rode o coamando 'dotnet add package Humanizer' no teminal do VSCode;
      - Então no AspNetCoreTodo.csproj, deve aparecer na referência a seguine linha:
      ```
      <PackageReference Include="Humanizer" Version="2.8.26" />
       ```
     - **Utilização:**
      - Para utilizar o package no código precisamos utilizar um "using"
      - Então em Views/Todo/Index.cshtml coloque:
      ```
       @model TodoViewModel
       @using Humanizer
      ```
      - E atualize a linha
       ```
      <td>@item.DueAt.()</td>
      para
      <td>@item.DueAt.Humanize()</td>
       ```
       - Se atualizarmos o navegador poderemos ver a diferença da forma que os dados estão sendo apresentados, pois agora ele não fala mais a hora e data mas sim, quanto tempo foi passado.
   - ##Uso de Banco de Dados
     - O bancos de dados pode se conectar com SQLServer, PostgreSQL e MySQL, mas também funciona com bancos de dados NoSQL como Mongo. Mas aqui usaremos SQLite neste projeto para tornar as coisas fáceis de configurar;
    - **Conectando ao Banco de Dados**
      - Vamos precisar de:
        - **1.Os pacotes do Entity Framework Core**: Eles estão incluídos por padrão em todos os projetos ASP.NET Core.
        - **2.Um banco de dados**. Pelo comando 'dotnet new mvc --auth Individual -o AspNetCoreTodo ' já é criado um pequeno banco de dados SQLite, na raiz do projeto chamado app.db;
        - **3.Uma classe de contexto de banco de dados**: O contexto do banco de dados é uma classe C # que fornece um ponto de entrada no banco de dados para que assim seu código poderá interagir com o banco de dados para ler e salvar itens. Já existe uma classe de contexto básica no arquivo Data/ApplicationDbContext.cs
        - **4.Uma string de conexão** Esteja você se conectando a um banco de dados de arquivos local (como SQLite) ou a um banco de dados hospedado em outro lugar, você definirá uma string que contém o nome ou endereço do banco de dados ao qual se conectar. Isso já está configurado por defautl no appsettings.jsonfile: a string de conexão para o banco de dados SQLite isDataSource = app.db.
       - 1. Entity Framework Core, usa o contexto do banco de dados, junto com a string de conexão, para estabelecer uma conexão com o banco de dados. Então vamos precisar dizer para o Entity Framework Core qual contexto, string de conexão e provedor de banco de dados escrendo o seguinte código no método ConfigureServices da classe Startup.cs:
       ```
       public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc();
            services.AddDbContext<ApplicationDbContext>(options =>
                options.UseSqlite(        
                    Configuration.GetConnectionString("DefaultConnection")));
                    ...
          }
       ```
  - Este código adiciona o ApplicationDbContext ao contêiner de serviço dizendo ao Entity Framework Core para usar o provedor de banco de dados SQLite, com a string de conexão da configuração (appsettings.json). O banco de dados está configurado e pronto para ser usado porém sem tabelas;

- ## Atualizando o Contexto
  - No arquivo Data/ApplicationDbContext.cs faça as modificações:
      ```
       public class ApplicationDbContext : IdentityDbContext
        {
           public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
                : base(options)
           {
            }

            public DbSet<TodoItem> Items { get; set; }
            protected override void OnModelCreating(ModelBuilder builder)
       {
           base.OnModelCreating(builder);
           // ...
     ```
  - Um DbSet representa uma tabela ou coleção no banco de dados. Ao criar uma propriedade DbSet <TodoItem> chamada Items, você está dizendo ao Entity Framework Core que deseja armazenar entidades TodoItem em um item chamado de tabela então para atualizar o banco de dados para refletir a alteração que foi feita é preciso criar uma migração.
- ## Criando uma Migration
  - Como não existe no banco de dados, precisamos criar uma migration para atualizar o banco de dados com o seguinte comando 'dotnet ef migrations add AddItems'. AddItems é o nome da migration.
  - No terminal deve retornar:
  **Build started...<br/>**
  **Build succeeded.<br/>**
  **Done. To undo this action, use 'ef migrations remove'<br/>**
  - Podemos verificar em Data/Migrations que alguns arquivos:
 
  ![alt text](https://raw.githubusercontent.comC:\Users\User\Desktop\LittleAspnet\LittleAspNetCoreBook_Portuguese\Images\migrations.png)
  
  
  
  

- ## Comandos: Usando o Git ou GitHub 
  - **Por segurança e facilidade de compartilhamento, entre outras funcionalidades é utilizado o Github, além disso ele serve como o seu curriculo de programador;**
  - 'cd ..' saia da pasta do projeto;
  - 'git init' inicia um novo repositório na pasta raiz do projeto. Caso ocorra erro volte nos dowloads e baixe e configure o Git Bash. Ele deve criar uma pasta .git.

- ## Termos:
  - **AddSingleton** adiciona seu serviço ao contêiner de serviço como um singleton. Isso significa que apenas uma cópia do  da classe FakeTodoItemService é criada e é reutilizada sempre que o serviço é solicitado.
  - **Arquitetura n-tier**: A maioria dos projetos maiores usa uma arquitetura de três camadas: uma camada de apresentação, uma camada de lógica de serviço e uma camada de repositório de dados. Um repositório é uma classe que é focada apenas no código do banco de dados (sem lógica de negócios). Neste aplicativo, você os combinará em uma única camada de serviço por simplicidade, mas fique à vontade para experimentar diferentes maneiras de arquitetar o código.
  - **Booleano** (valor verdadeiro / falso), Por padrão, será falso para todos os novos itens. Posteriormente, pode-se mudar essa propriedade para true quando o usuário clicar na caixa de seleção de um item na visualização.
  - **get; set; ou (getter e setter)** leitura / gravação.
  - **Guids (orGUIDs)** são longas sequências de letras e números, como 43ec09f2-7f70-4f4b-9559-65011d5781bb. Como os guias são aleatórios e é improvável que sejam duplicados acidentalmente, eles são comumente usados como IDs únicos. Você também pode usar um número (inteiro) como ID da entidade do banco de dados, mas precisará configurar seu banco de dados para sempre aumentar o número quando novas linhas forem adicionadas ao banco de dados.
  - **DueAt** é um DateTimeOffset, que é um tipo de C# que armazena um carimbo de data/hora junto com um deslocamento de fuso horário do UTC. Armazenar o deslocamento de data, hora e fuso horário juntos facilita o agendamento de datas com precisão em sistemas em fusos horários diferentes. Além disso temos o "?" ponto de interrogação após o tipo DateTimeOffset ? Essa marca a propriedade DueAt como anulável ou opcional. Se o "?" não foi incluído, todos os itens de pendências precisam ter uma data de vencimento.
  - **mapeador objeto-relacional (ORM)** torna mais fácil escrever códigos que interagem com um banco de dados, adicionando uma camada de abstração entre seu código e o próprio banco de dados. Hibernate em Java e ActiveRecord inRuby são dois ORMs bem conhecidos.Ainda existem vários ORMs para .NET, incluindo um criado pela Microsoft e incluído no ASP.NET Core por padrão: Entity Framework Core. EntityFramework Core torna mais fácil conectar-se a vários tipos de bancos de dados diferentes e permite usar o código C# para criar consultas de banco de dados que são mapeadas de volta para modelos C# (POCOs).
  - **Migrations** controlam as mudanças na estrutura do banco de dados ao longo do tempo, possibilitando reverter um conjunto de mudanças, ou criar um segundo banco de dados com a mesma estrutura do primeiro. Com as migrations, você tem um histórico completo de modificações, como adicionar ou remover colunas (e tabelas inteiras);
  - **Required** informa que o campo é obrigatório ao ASP.NET Core que essa sequência não pode ser nula ou vazia.
  - **Strings** em C# são sempre anuláveis, portanto, não há necessidade de marca-lás como anulável. As strings C # podem ser nulas, vazias ou conter texto.
  - **SQLite** é um gerenciador banco de dados leve que não exige nenhuma instalação de ferramenta para pode ser executado
  - **Tag Helpers(tags de ajuda)**: Antes que a visualização seja renderizada, o ASP.NET Cor substitui esses auxiliares de tag por atributos HTML reais, onde o ASP.NET Core o gera para você automaticamente. Exemplos: Os atributos asp-controller e asp-action no elemento <a>.
  - **Using** são instruções que se encontrão na parte superior do arquivo para importaras informações de outras classes, e evitar mensagens de erros como: "The type or namespace name 'TodoItem' could not be found (are you missing a using directive or an assembly reference?)".
 
 
 
 
  
 
 
  
 
