# Guia de boas práticas

## Sumário
- [Visão Geral]()
    - [Princípios]()
    - [Diretrizes]()
    - [Mandamentos]()
- [IDE]()
- [Configuração do ambiente de desenvolvimento em README.md]()
- [Persistência de dados]()
    - [Considerações Gerais]()
    - [SaaS, cloud-hosted or auto-hospedado?]()
    - [Soluções de Persistência]()
        - [RDBMS]()
        - [NoSQL]()
        - [Armazenamento de coleções]()
        - [Armazenamento key-value]()
        - [Gráfico de banco de dados]()
- [Ambientes]()
    - [Desenvolvimento local]()
    - [Integração contínua]()
    - [Teste]()
    - [Estadiamento]()
    - [Produção]()

- [Créditos]()
- [Licença]()


# Visão Geral

Estes guias foram criados para padronizar todo o workflow de desenvolvimento na Mkt Virtual. Do código ao deploy, eles cobrem cada parte do nosso trabalho diário. 

Nós não queremos nos limitar a certas plataformas ou estruturas de tecnologia. Diferentes problemas requerem soluções diferentes e, portanto, essas diretrizes são válidas para várias arquiteturas.

_* Esta é uma versão em desenvolvimento._

## Princípios

O desenvolvimento na Mkt Virtual é guiado por alguns princípios. Todas os guias servem para que sigamos estes princípios fundamentais:

- **Legibilidade** - esse é um dos mais importantes. Antes de computadores lerem nossos códigos, desenvolvedores lerão. Ter um código legível é imprescindível para manter a equipe produtiva e os projetos o mais lisos possível.
- **Organização** - em um ambiente organizado, trabalhamos melhor e com menos stress. Algumas regras e boas práticas nos ajudam a organizar nosso código e todo o projeto.
- **Simplicidade** - nosso código e nossos projetos devem ser o mais simples possível, nunca menos ou mais que isso. Ambientes simples facilitam a entrada de novos integrantes e também a escalabilidade de cada projeto.

## Diretrizes

- Seja consistente.
- Não reescreva código existente para seguir estes guias.
- Não viole diretrizes sem uma boa razão.
- Uma razão é boa quando você pode convencer alguém da equipe.
- Leia todos os anexos indicados por "(?)".

## Mandamentos

1. README.md na raíz do diretório
2. Comando único para dar run
3. Comando único para dar deploy
4. Builds recriaveis
5. Dependências da Build documentados na ["Lista de Materiais"]()
6. Use UTC as the timezone all around 

# IDE

Na Mkt Virtual, podemos usar qualquer editor. Vim, SublimeText, Brackets, TextMate ou outros. Nós usamos o EditorConfig para manter o código consistente e seguindo as diretrizes deste guia. As regras estão no arquivo .editorconfig.

Instale plugins para Vim, SublimeText, Brackets, TextMate ou outro editor.

# Configuração do ambiente de desenvolvimento em README.md

Documentar todas as partes do ambiente de desenvolvimento/producao. Esforce-se para utilizar a mesma configuração e versões em todos os ambientes, a partir dos ambientes de desenvolvimento e terminando com o ambiente de produção real. Isso inclui o banco de dados, servidor de aplicativos, servidor proxy (nginx, Apache, ...), versões SDK, gems/libraries/modules.

Automatize o processo de instalação tanto quanto possível. Por exemplo, o [Docker Compose](https://docs.docker.com/compose/) pode ser usado tanto na produção como no desenvolvimento para configurar um ambiente completo, onde os arquivos [Docker]() recuperam todas as partes do software e contam os scripts necessários para configurar o ambiente e todas as partes dele. Considere o uso de cópias arquivadas dos instaladores, caso os pacotes upstream mais tarde não estejam disponíveis. Uma precaução mínima é manter uma soma de verificação SHA-1 dos pacotes e certificar-se de que a soma de verificação coincide quando os pacotes estão instalados.

Considere armazenar quaisquer partes relevantes do ambiente de desenvolvimento e suas dependências em algum armazenamento persistente. Se o ambiente pode ser construído usando o [Docker](), uma maneira possível de fazer isso é usar a exportação do docker.

# Persistência de dados

## Considerações gerais

Independente da solução de persistência que seu projeto usa, existem considerações gerais que você deve seguir:

- Ter backups que são verificados para funcionar
- Tem scripts ou outras ferramentas para copiar dados persistentes de um env para outro, p. Ex. de produção para homologação  para depurar algo
- Existem planos para implementar as atualizações da solução de persistência (por exemplo, atualizações de segurança do servidor de banco de dados)
- Planejamento para aumentar a solução de persistência
- Planejamento ou ferramentas para gerenciar alterações de esquema
- Monitoramento no local para verificar a saúde da solução de persistência

## SaaS, cloud-hosted ou auto-hospedado?

Uma escolha importante em relação a qualquer solução é onde executá-lo.

- **SaaS** - rápido para começar, fácil de ampliar, algum trabalho de infra-estrutura necessário para permitir o acesso de todos os lugares etc.
 - **Auto-hospedado na nuvem** - permite o banco de dados de sintonia mais do que o SaaS e provavelmente mais barato na escala em termos de hospedagem, mas mais intensivo em mão-de-obra
 - **Auto-hospedado em hardware próprio** - capaz de ajustar tudo e gerenciar a segurança física, mas mais caro e trabalhista intensivo

## Soluções de Persistência

Esta seção pretende fornecer algumas orientações para selecionar o tipo de solução de persistência. A escolha sempre precisa ser adaptada ao problema e nenhuma dessas é uma bala de prata, no entanto.

### RDBMS

Escolha um sistema de banco de dados relacional, como o PostgreSQL, quando a integridade dos dados e das transações é uma grande preocupação ou quando é necessária muita análise de dados. As funções de conformidade, agregação e transformação ACID do RDBMS ajudarão.

### NoSQL

Escolha um banco de dados NoSQL quando você espera escalar horizontalmente e quando não precisar de ACID. Escolha um sistema que se encaixa no seu modelo.

### Armazenamento de coleções

Armazena documentos que podem ser facilmente abordados e pesquisados ​​por conteúdo ou por inclusão em uma coleção. Isso é possível porque o banco de dados entende o formato de armazenamento. Use para isso: armazenar um grande número de documentos estruturados. Exemplos notáveis:

- CouchDB
- ElasticSearch
- MongoDB
> *Observe que, desde a versão 9.4, o PostgreSQL também pode ser usado para armazenar JSON de forma nativa.*

### Armazenamento key-value (chaves)

Armazenar chaves, ou às vezes grupos de pares chave-valor, acessíveis por chave. Considera que os valores são simplesmente blobs, portanto, não fornece os recursos de consulta das lojas de documentos. Tamanhos escaláveis ​​para imensos. Exemplos notáveis:

- [Cassandra](http://cassandra.apache.org/)

### Gráfico de banco de dados

As bases de dados de gráficos gerais armazenam nós e bordas de um gráfico, fornecendo pesquisas sem índice dos vizinhos de qualquer nó. Para aplicações onde as consultas semelhantes a um gráfico, como o caminho mais curto ou o diâmetro, são cruciais. Também existem bases de dados de grafos especializados para armazenar, e. Triplos RDF.

# Ambientes

Esta seção descreve os ambientes que você deveria ter, no mínimo. Pode parecer muito, mas existe um propósito para cada um.

- Desenvolvimento local
- Integração contínua
- Teste
- Homologação
- Produção


## Ambiente de desenvolvimento local

Este é o seu ambiente de desenvolvimento local. Você provavelmente não deveria ter um ambiente de desenvolvimento externo compartilhado. Em vez disso, você deve trabalhar para tornar possível executar todo o sistema localmente, esfaqueando ou zombando de serviços de terceiros, conforme necessário.

## Ambiente de integração contínua

A CI é (entre outras coisas) para se certificar de que o software compilado e os testes automatizados passam após cada mudança.

## Ambiente de teste

Este é um ambiente compartilhado em que o código é implantado com a maior freqüência possível, de preferência, cada código de tempo está comprometido com o ramo principal. Pode ser quebrado de tempos em tempos, especialmente na fase de desenvolvimento ativo. É um importante ambiente canário e é tão parecido com a produção quanto possível. Todas as integrações externas estão configuradas para usar versões de nível de teste de outros serviços.

## Ambiente de homologação

O cenário é configurado exatamente como a produção. Nenhuma alteração no ambiente de produção ocorre antes de ter sido ensaiada aqui primeiro. Qualquer problema misterioso de produção pode ser depurado aqui.

## Ambiente de produção

A máquina. Registrando erros e avisos (logs), monitorado, limpo periodicamente, quadrado e protegido.

# Lista de Materiais

Este documento deve ser incluído em todos os artefatos de construção e deve conter o seguinte:

- Que versão (s) de um SDK e ferramentas críticas foram usadas para produzi-lo
- Quais dependências foram incluídas
- Um número de revisão globalmente exclusivo da compilação (ou seja, um hash SHA-1 git)
- O ambiente e as variáveis utilizadas na construção do pacote
- Uma lista de testes ou verificações com falha

# Segurança

Esteja ciente de possíveis ameaças e problemas de segurança. Você deve, pelo menos, estar familiarizado com as vulnerabilidades do [OwASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) e você deve monitorar vulnerabilidades em qualquer software de terceiros que você use.

Boas diretrizes de segurança genéricas seriam:

## Credenciais

Nunca envie credenciais não criptografadas em rede pública. Use sempre criptografia (como HTTPS, SSL, etc.).

## Segredos

Nunca armazene segredos (senhas, chaves, etc.) nas fontes no controle de versão! É muito fácil esquecer que eles estão lá e a fonte do projeto tende a acabar em muitos lugares (máquinas de desenvolvimento, servidores de teste de desenvolvimento, etc.), o que aumenta desnecessariamente o risco de comprometimento de um importante segredo. Além disso, o controle de versão tem a característica desagradável de substituir permissões de arquivos, portanto, mesmo que você segure suas permissões de arquivo de configuração, a próxima vez que você verificar a origem, as permissões serão substituídas pelo padrão legível por público.

Provavelmente, a maneira mais fácil de lidar com segredos é colocá-los em um arquivo separado nos servidores que precisam deles e ser ignorado pelo controle de versão. Você pode manter, e um arquivo .sample no controle de versão, com valores falsos para ilustrar o que deve ir lá no arquivo real. Em alguns casos, não é fácil incluir um arquivo de configuração separado da configuração principal. Se isso acontecer, considere usar variáveis ​​de ambiente ou escrever o arquivo de configuração de um modelo controlado por versão na implantação.

## Login Throttling

Coloque limites na quantidade de tentativas de login permitidas por cliente por unidade de tempo. Bloqueie uma conta de usuário para o tempo específico após um determinado número de tentativas falhadas (por exemplo, bloqueio por 5 minutos após 20 tentativas de login com falha). O objetivo dessas medidas é tornar infaáveis ​​os ataques de força bruta em linha contra nomes de usuários/senhas.

## Armazenamento de senha do usuário

Nunca armazene senhas em texto simples!
Nunca armazene senhas em forma criptografada reversível, a menos que seja absolutamente exigido pelo aplicativo/sistema. Aqui está um bom artigo sobre o que e o que não fazer: [https://crackstation.net/hashing-security.htm](https://crackstation.net/hashing-security.htm)

Se você precisa obter senhas de texto simples do banco de dados, aqui estão algumas sugestões a serem seguidas.

Se as senhas não forem convertidas novamente em texto simples (por exemplo, um procedimento especial é necessário), mantenha as chaves de descriptografia longe do aplicativo que acessa o banco de dados regularmente.

Se as senhas ainda precisam ser regularmente descriptografadas, separe a funcionalidade de descriptografia da aplicação principal, tanto quanto possível - por exemplo. um servidor separado aceita solicitações para descriptografar uma senha, mas impõe um maior nível de controle, como aceleração, autorização, etc.

Sempre que possível (deve ser na grande maioria dos casos), armazene senhas usando um bom hash unidirecional com um bom sal aleatório. E, não, SHA-1 não é uma boa escolha para uma função hash neste contexto. As funções Hash que são projetadas com senhas em mente são deliberadamente mais lentas, o que torna os ataques de força bruta off-line mais demorados, portanto, menos viáveis. Veja esta publicação para obter mais detalhes: [http://security.stackexchange.com/questions/211/how-to-securely-hash-passwords/31846#31846](http://security.stackexchange.com/questions/211/how-to-securely-hash-passwords/31846#31846)

## Registro de auditoria

Para aplicações que manipulam dados confidenciais, especialmente quando certos usuários têm permissão para acesso ou controle relativamente amplo, é bom manter algum tipo de log de auditoria - armazenar uma seqüência de ações/eventos que ocorreram no sistema, juntamente com o evento/fonte originador (usuário, trabalho de automação, etc.). Isto pode ser, por exemplo:

```
2012-09-13 03:00:05 Job "daily_job" performed action "delete old items".
2012-09-13 12:47:23 User "admin_user" performed action "delete item 123".
2012-09-13 12:48:12 User "admin_user" performed action "change password of user foobar".
2012-09-13 13:02:11 User "sneaky_user" performed action "view confidential page 567".
...
```

O log pode ser um arquivo de texto simples ou armazenado em um banco de dados. Pelo menos esses três itens são bons para ter: um timestamp exato, o criador de ação/evento (quem fez isso) e a ação/evento real (o que foi feito). As ações exatas a serem registradas dependem do que é importante para a própria aplicação, é claro.

O log de auditoria pode ser parte do registro normal de aplicativos, mas a ênfase aqui é no registro que fez o que e não apenas uma determinada ação foi realizada. Se possível, o log de auditoria deve ser feito à prova de inviolabilidade, p. Ex. só pode ser acessado por um processo de log dedicado ou usuário e não diretamente pelo aplicativo.

## Ação suspeita Estruturação e/ou bloqueio

Isso pode ser visto como uma generalização do Login Throttling, desta vez introduzindo mecanismos similares para ações arbitrárias que são consideradas "suspeitas" no contexto da aplicação. Por exemplo, um sistema ERP que permite aos usuários normais acessar uma quantidade substancial de informações, mas espera que os usuários se preocupem apenas com um pequeno subconjunto dessa informação, pode limitar as tentativas de acessar conjuntos de dados maiores do que o esperado com muita rapidez. Por exemplo. impedir que os usuários baixem a lista de todos os clientes, se os usuários deveriam trabalhar em um ou dois clientes ao mesmo tempo. Observe que isso é diferente de limitar o acesso completamente - os usuários ainda podem obter informações sobre qualquer cliente, simplesmente não todos eles ao mesmo tempo. Dependendo do sistema, a aceleração pode não ser suficiente, por exemplo. quando invoca uma ação em todos os recursos com um único pedido. Em seguida, o bloqueio pode ser necessário. Observe a diferença entre fazer 1000 solicitações em 10 segundos para recuperar informações completas do cliente, um cliente por vez e fazer um único pedido para recuperar essas informações de uma só vez.

O que é suspeito aqui depende fortemente do uso esperado da aplicação. Por exemplo. em um sistema, a exclusão de 10000 registros pode ser uma ação completamente legítima, mas não em outro.

## Dados anônimos

Sempre que grandes conjuntos de dados são exportados para terceiros, os dados devem ser anônimos, tanto quanto possível, dado o uso pretendido dos dados. Por exemplo, se um serviço de terceiros fornecerá análise estatística geral em um banco de dados de clientes, provavelmente não precisa conhecer os nomes, endereços ou outras informações pessoais para clientes individuais. Mesmo um número genérico de identificação do cliente pode ser muito revelador, dependendo do conjunto de dados. Dê uma olhada neste artigo: http://arstechnica.com/tech-policy/2009/09/sua-secrets-live-online-in-databases-of-ruin/.

Evite registrar informações de identificação pessoal, por exemplo, o nome do usuário.

Se os seus registros contiverem informações confidenciais, certifique-se de saber como os logs são protegidos e onde eles estão localizados também no caso de sistemas hospedados de gerenciamento de log hospedado.

Se você deve registrar informações confidenciais, tente o hash antes do log para que você possa identificar a mesma entidade entre diferentes partes do processamento.

## Armazenamento temporário de arquivos

Verifique se você está ciente de onde seu aplicativo está armazenando arquivos temporários. Se você estiver usando diretórios publicamente acessíveis (o que é provavelmente o padrão), como/tmp e/var/tmp, certifique-se de criar seus arquivos com o modo 600, de modo que eles sejam legíveis apenas pelo usuário em que seu aplicativo esteja funcionando como. Alternativamente, tenha um diretório protegido para armazenar arquivos temporários (diretório acessível apenas pelo usuário do aplicativo).

## Ambiente do servidor dedicado versus compartilhado

As ameaças de segurança podem ser bastante diferentes, dependendo se o aplicativo vai ser executado em um ambiente compartilhado ou dedicado. O compartilhado aqui significa que existem outros aplicativos (não necessariamente de terceiros) que estão sendo executados no mesmo servidor. Nesse caso, ter permissões de arquivo apropriadas torna-se crítica, caso contrário o código fonte do aplicativo, arquivos de dados, arquivos temporários, logs, etc. podem ser acessados ​​por usuários não planejados. Em seguida, uma violação de segurança em um aplicativo de terceiros pode resultar em comprometimento da sua aplicação.

Você nunca pode ter certeza do tipo de ambiente que sua aplicação executará durante todo o seu tempo de vida - pode começar em um servidor dedicado, mas com o passar do tempo, aplicativos de terceiros podem ser adicionados ao mesmo sistema. É por isso que é melhor planejar desde o primeiro momento em que o aplicativo é executado em um ambiente compartilhado e tomar todas as precauções. Aqui está uma lista não exaustiva dos arquivos/diretórios sobre os quais você precisa pensar:

- código fonte do aplicativo
- diretórios de dados
[ diretórios temporários de armazenamento (geralmente, por padrão, o sistema wide/tmp pode ser usado - veja acima)
- arquivos de configuração
- diretórios de controle de versão - .git, .hg, .svn, etc.
- scripts de inicialização (podem conter variáveis ​​de inicialização, segredos, etc.)
- arquivos de log
- despejos de acidentes
- chaves privadas (SSL, SSH, etc.)
- etc.

Às vezes, alguns arquivos precisam ser acessados ​​por diferentes usuários (por exemplo, conteúdo estático veiculado por apache). Nesse caso, tome cuidado para permitir apenas o acesso ao que realmente é necessário.

Tenha em mente que, em um sistema de arquivos UNIX/Linux, o acesso de gravação a um diretório é muito poderoso para permissão - ele permite que você exclua arquivos nesse diretório e recrie-os (o que resulta em um arquivo modificado)./tmp e/var/tmp são, por padrão, protegidos contra este efeito, devido ao bit pegajoso que deve ser configurado nesses.

Além disso, como mencionado na seção de segredos, as permissões de arquivo podem não ser preservadas no controle de versão, portanto, mesmo se você as configurou uma vez, a próxima verificação/atualização/o que pode substituí-las. Uma boa idéia é então ter um Makefile, um script, um gancho de controle de versão ou algo semelhante que definisse as permissões corretas ao atualizar as fontes.

## Monitoramento de aplicativos

O monitoramento do status completo de um serviço exige que as verificações de monitoramento de nível de sistema e de controle de aplicativos sejam executadas. As verificações de nível de sistema operacional incluem, por exemplo, uso de CPU, disco ou memória, processos em execução, portas abertas, etc. As verificações específicas do aplicativo são, no entanto, as mais importantes do ponto de vista do serviço executável. Isso pode ser qualquer coisa de "esse URL responde e retorna o status HTTP 200", para verificar a conectividade do banco de dados, a consistência dos dados e assim por diante.

Esta seção descreve uma maneira de implementar as verificações específicas do aplicativo, o que tornaria mais fácil monitorar a saúde global do aplicativo e dar controle total aos desenvolvedores de aplicativos para determinar quais verificações são significativas no contexto da aplicação concreta.

Em essência, a idéia é ter um único ponto final (um URL de aplicação) que possa dar uma boa visão geral do status de toda a aplicação. Isso é implementado dentro do aplicativo e requer trabalho da equipe do projeto, mas, por outro lado, a equipe do projeto é a pessoa que realmente pode definir o que é um estado OK do aplicativo e o que é considerado um estado ERROR.

O aplicativo poderia implementar qualquer número de verificações do "subsistema". Por exemplo,

a conexão com o banco de dados está subindo
os dados estão em um estado consistente (por exemplo, uma lista de itens em uma determinada tabela de banco de dados é significativa)
Os serviços de terceiros a que o aplicativo se integra são acessíveis
Os índices ElasticSearch estão em um estado consistente
Qualquer outra coisa que faça sentido para a aplicação
Um status de visão geral combinada deve ser fornecido pelo aplicativo, agregando as informações das várias verificações do subsistema. A idéia é que um sistema de monitoramento externo pode rastrear apenas essa visão geral combinada, de modo que o monitoramento externo não precisa ser reconfigurado quando uma nova verificação de aplicativo é adicionada ou modificada. Além disso, os desenvolvedores são os que podem decidir sobre o que o status geral é baseado em verificações de subsistema (por exemplo, quais são críticas, enquanto não são, etc.).

## Página de status

Todas as verificações de status DEVEM estar acessíveis em/URL de status da seguinte maneira:

- /status - a página de status geral (obrigatória)
- /status/subsystem1 - uma verificação de status para o subsistema de especificações (opcional)
- ...

A página principal/status deve, no mínimo, fornecer um status geral do sistema, conforme descrito na próxima seção. Isso significa que a página principal/status deve executar TODAS as verificações do subsistema e relatar o estado geral do sistema agregado. Cabe aos desenvolvedores decidir como o status geral do sistema é determinado com base nos subsistemas. Por exemplo, um estado de ERRO de algum subsistema não crítico só pode gerar um estado de AVISO geral.

Por razões de desempenho, algumas verificações do subsistema podem ser excluídas desta página geral/status - por exemplo, quando a verificação provoca maior uso de recursos, leva mais tempo para concluir, etc. No geral, a página de status principal deve ser suficientemente leve para que possa ser consultado com relativa frequência (cada 1-3 minutos) e não causar demasiada carga no sistema. As verificações de subsistema que estão excluídas da verificação de status geral devem ter seus próprios URLs, como mostrado acima. Naturalmente, o monitoramento desses exigiria modificações na configuração do sistema de monitoramento. Para superar isso, uma abordagem diferente pode ser tomada: o aplicativo pode executar as verificações do subsistema pesado em um processo em segundo plano a uma taxa aceitável e armazenar o status internamente. Isso permitiria que a página de status principal refletisse também essas verificações pesadas (por exemplo, recuperaria o último status de verificação executada). Esta abordagem deve ser utilizada, a menos que sua implementação seja muito difícil.

## Formato da página de status

Nós propomos dois formatos alternativos para as páginas de status - planície e JSON.

### Formato simples

O formato simples possui um status por linha na chave de formulário: valor. A chave é um nome de subsistema/verificação e o valor é o valor de status. O valor de status pode ser um dos seguintes:

- `OK`
- `WARN Message`
- `ERROR Message`

Onde a `Mensagem` pode ser um texto significativo, que pode ajudar a identificar rapidamente o problema. A mensagem é de uma única linha, sem restrição de comprimento especificada, mas use o senso comum - e. provavelmente não deve ter mais de 200 caracteres.

A página de status principal DEVE ter uma chave chamada `status` que mostra o status total do aplicativo agregado. As linhas de status de verificação do subsistema individual são opcionais. As chaves de estado do subsistema devem ter um sufixo `_status`. aqui estão alguns exemplos:

Quando tudo está bem:

```
status: OK
database_status: OK
elastic_search_status: OK
```

Quando algum cheque está falhando:

```    
status: ERROR o banco de dados não está acessível
database_status: ERROR Connection failed
elastic_search_status: OK
```

Falhas múltiplas ao mesmo tempo:

```
status: ERROR falha nos subsistemas: database, elasticsearch. Para detalhes, veja https://myapp.example.com/status
database_status: ERROR Connection failed
elastic_search_status: WARN Demasiadas entradas no índice A.
```

Além das linhas de status, uma página de status pode ter chaves não-status. Por exemplo, aqueles podem mostrar algumas métricas (que podem ou não ser monitoradas). As chaves adicionais devem ser prefixadas com o nome do subsistema.

```
status: OK
database_status: OK
clientes do banco de dados: 378
database_items: 8934748
elastic_search_status: OK
elastic_search_shards: 20
```

O status geral pode, naturalmente, ser baseado em algumas das métricas:

```
status: WARN Demasiados itens no banco de dados
database_status: WARN Demasiados clientes no banco de dados
clientes do banco de dados: 378
database_items: 1
elastic_search_status: OK
elastic_search_shards: 20
```

As verificações do subsistema que têm seu próprio URL (/ status/subsystemX) devem seguir um formato similar, ter um status de chave obrigatório e várias teclas adicionais opcionais. Exemplo para e./status/database:

```
status: OK
connection_pool: 30
latência: 2
```

### Formato JSON

O formato JSON das páginas de status pode ser muitas vezes preferível, por exemplo, quando a ferramenta ou a integração com outros sistemas é mais fácil de conseguir através de um formato de dados comum.

Os valores de status seguem o mesmo formato descrito acima - OK, WARN Message e ERROR Message.

O equivalente à chave de status do formato simples é uma chave de status no objeto raiz JSON. Os subsistemas devem usar objetos aninhados também com uma chave de status obrigatória. aqui estão alguns exemplos:

Está tudo bem:

```
{
    "status": "OK",
    "database": {
        "status": "OK"
    },
    "elastic_search": {
        "status": "OK"
    }
}
```

Página de status com métricas adicionais:

```
{
    "status": "OK",
    "uptime": 18234,
    "database": {
        "status": "OK",
        "connection_pool": 30
    },
    "elastic_search": {
        "status": "OK",
        "multinode": false
    }
}
```

Algo falhando:

```
{
    "status": "ERROR Database is not accessible. See https://myapp.example.com/status for details.",
    "database": {
        "status": "ERROR Connection failed",
        "connection_timeout": 30
    },
    "elastic_search": {
        "status": "OK"
    }
}
```

## Códigos de status HTTP

Sempre que o estado geral do aplicativo estiver OK, o código de status HTTP na resposta da página de status DEVE ser configurado para 200 (OK). Caso contrário, um código de erro 5XX DEVE ser configurado. Por exemplo, o código 500 (Internal Server Error) pode ser usado. Opcionalmente, o status de WARN não crítico ainda pode responder com 200.

## Verificações de saúde do Load Balance

Muitas vezes, o aplicativo está sendo executado atrás de um Load Balance. Os balanceadores de carga geralmente podem monitorar servidores de aplicativos ao pesquisar um determinado URL. A verificação de integridade é usada para que o Load Balance possa parar de rotear o tráfego para os servidores de aplicativos com falha.

A página geral/status é um bom candidato para o URL de verificação de saúde do Load Balance. No entanto, uma página de status dedicada separada para uma verificação de saúde do Load Balance fornece um benefício importante. Essa página pode ser ajustada para quando o aplicativo é considerado saudável da perspectiva do Load Balance. Por exemplo, um erro em um subsistema ainda pode ser considerado um erro crítico para o status geral do aplicativo, mas não necessariamente é necessário que o servidor do aplicativo seja removido do pool do Load Balance. Um bom exemplo é uma verificação de status de integração de terceiros. A página de verificação de saúde do Load Balance deve retornar apenas código de status não-200 quando a instância do aplicativo deve ser considerada não operacional.

A página de verificação de saúde do Load Balance deve ser colocada em um URL/status/health. Dependendo do seu Load Balance, o formato dessa página pode se desviar do formato de status geral descrito aqui. Alguns balanceadores de carga podem até observar apenas o código de status HTTP retornado.

## Controle de acesso

As páginas de status podem precisar de uma autorização adequada, especialmente no caso de exporem informações de depuração em mensagens de status ou métricas de aplicativos. A autenticação básica HTTP ou as restrições baseadas em IP geralmente são bons candidatos suficientes para considerar.

# Lista de verificação

Para evitar esquecer as coisas mais importantes, aqui estão algumas listas de verificação úteis para seus projetos atuais ou futuros.

Lista de verificação de responsabilidade

Em projetos maiores, especialmente quando participam múltiplos partidos, é crucial acompanhar todos os diferentes aspectos e suas responsabilidades. A tabela a seguir ilustra como uma lista de verificação para liberar um site pode parecer:

| Aspecto    | Tarefa                              | Responsável pessoa / nipe | Deadline     | Status            |
|---        |---                                |---                         |---           |---                |
| Frontend  | Website wireframes                | e.g. MKT / Bomfim  | e.g. 17.Jun   |  e.g. in progress |
| Frontend  | Website design                    | e.g. MKT / G.Verona  | e.g. 23.Jul   |  e.g. waiting     |
| Frontend  | Website templates                 |   |   |   |
| Frontend  | Content creation and population   |   |   |   |
| Backend   | Setup CMS                         |   |   |   |
| Backend   | Setup staging environment         |   |   |   |
| Backend   | Setup production environment      |   |   |   |
| Backend   | Migrate hosting services to client accounts |   |   |   |
| Backend   | DNS configuration                 |   |   |   |
| Backend   | Setup website analytics           |   |   |   |
| Backend   | Integrate marketing automation    |   |   |   |
| Backend   | Web font license                  |   |   |   |
| Dates     | Website/Product go-live time      |   |   |   |
| Dates     | Publish the website               |   |   |   |

# Lista de verificação de lançamento

Quando você estiver pronto para lançar, lembre-se de verificar tudo em sua lista de verificação de lançamento! A resultante paz de espírito, repetibilidade e confiabilidade é uma ótima vantagem.

Você tem um, certo? Se você não, aqui está um bom ponto de partida genérico para você:

* [ ] A implantação funciona da mesma maneira, independentemente do ambiente em que você estiver implantando.
* [ ] Todos os ambientes têm nomes bem definidos, e eles são referidos usando esses nomes
* [ ] Todos os ambientes têm a mesma pilha de software subjacente
* [ ] Toda a configuração do ambiente é controlada pela versão (configuração do servidor web, scripts de compilação CI etc.)
* [ ] O produto foi testado a partir das redes de onde será usado (por exemplo, Internet pública, LAN do cliente)
* [ ] O produto foi testado com todos os dispositivos segmentados
* [ ] Existe uma maneira simples de descobrir qual código está sendo executado em qualquer ambiente dado
* [ ] Um esquema de versão foi definido
* [ ] Qualquer versão do produto deve ser facilmente mapeável para um estado da base do código
* [ ] Reverter uma implantação é possível
* [ ] Os backups estão em execução
* [ ] A restauração de um backup foi testada
* [ ] Nenhum segredo é armazenado no controle de versão
* [ ] O registro está ativado
* [ ] Existe um processo bem definido para acessar e pesquisar através de logs
* [ ] O registro inclui exceções e rastreios de pilha onde apropriado
* [ ] Os erros podem ser mapeados para rastreios de pilha
* [ ] Notas de lançamento foram escritas
* [ ] Os ambientes do servidor estão atualizados
* [ ] Existe um plano para atualizar os ambientes do servidor
* [ ] O produto foi testado por carga
* [ ] Existe um método para replicar o estado de um ambiente em outro (por exemplo, copiar prod para QA para reproduzir um erro)
* [ ] Todos os processos de lançamento repetidos foram automatizados

# Questões gerais a considerar

- Qual é a duração esperada / necessária do projeto?
- O projeto é único, ou haverá um desenvolvimento contínuo?
- Qual é o ciclo de lançamento para uma versão do serviço?
- Que ambientes (dev, test, staging, prod, ...) serão configurados?
- Como o tempo de inatividade do serviço de produção afetará o valor do serviço?
- Quão madura é a tecnologia? São esperadas mudanças importantes que quebram a compatibilidade com versões anteriores?

# Ferramentas úteis comprovadas

- [HTTPie](https://github.com/jakubroztocil/httpie) é uma ótima ferramenta para testar APIs na linha de comando. É simples passar em cabeçalhos e cookies personalizados, e ele ainda tem suporte de sessão.
- [jq](http://stedolan.github.io/jq/) é um processador CLI JSON. Massagem JSON dados provenientes de cURL (ou, claro, HTTPie!) À vontade. Outra ótima ferramenta para testes ou exploração da API.

# Créditos

Guias é mantido pela [Mkt Virtual](http://mktvirtual.com.br/). Ele segue conselhos de especialistas, dicas de grandes profissionais e opiniões de desenvolvedores apaixonados pelo que fazem.

Esta é uma adaptação de diversos guias para a nossa realidade. Algumas referências:

- [Guides](https://github.com/thoughtbot/guides) por [thoughtbot](https://github.com/thoughtbot)
- [CSS Guidelines](https://github.com/csswizardry/CSS-Guidelines) por [csswizardry](https://github.com/csswizardry)
- [Javascript](https://github.com/airbnb/javascript) por [airbnb](https://github.com/airbnb)
- [Backend best pratices](https://github.com/futurice/backend-best-practices) por [Ilkka Poutanen](https://github.com/ilkka)

# Licença

Guias foi criado pela equipe da Mkt Virtual. Ele é uma adaptação de [Guides, por thoughtbot, inc](https://github.com/thoughtbot/guides) e outros guias distribuídos livremente (referênciados devidamente em cada página). Ele é distribuído sob a [licença MIT](https://github.com/mktvirtual/guias/tree/master/LICENSE).