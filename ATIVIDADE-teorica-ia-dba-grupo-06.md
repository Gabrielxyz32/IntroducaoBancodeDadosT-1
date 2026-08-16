# Atividade Teórica: Usuários Especialistas, IA e Distribuição Segura de Dados

**Aluno(s):** Gabriel, Geovana, Luan, Murilo, Rebeca
**Turma:** Banco de Dados 2026
**Data:** 14/08/2026
**Repositório Git:** https://github.com/Gabrielxyz32/IntroducaoBancodeDadosT-1

## Resumo Executivo

Breve descrição do tema e da posição adotada pelo grupo.

## 1. Desenvolvimento Teórico

### 1.1 O que é o DBA e quais suas funções?

**Estrutura e Organização** 

O DBA ou Administrador do banco de dados, tem a função principal de garantir que o banco de dados esteja disponível, seguro, íntegro, rápido e recuperável em caso de problemas. Uma forma simples de entender: O desenvolvedor cria o sistema. O DBA garante que os dados por trás desse sistema estejam funcionando corretamente.

Exemplo de uma empresa: Imagine uma empresa que possui um sistema de vendas.
O sistema possui, clientes, produtos, pedidos, pagamentos, funcionários e estoque. Tudo isso fica armazenado no PostgreSQL (um sistema de gerenciamento de banco de dados) que precisa de um administrador.

O DBA pode participar da definição da estrutura do banco, e também verifica coisas como:

- se a tabela está bem estruturada
- se existem chaves primárias
- se os relacionamentos estão corretos
- se os tipos de dados são adequados
- se existem constraints para evitar dados inválidos

Por exemplo, se um desenvolvedor cria:
cliente, id, nome, email<br>
Mas, o DBA percebe que o email não deve ser duplicado. Então o que pode ser feito é:<br>
ALTER TABLE clientes<br>
ADD CONSTRAINT clientes_email_unique UNIQUE (email)<br>
Assim, o próprio banco impede dois clientes com o mesmo e-mail.

**Performance** 

O DBA é um dos responsáveis por otimização de programas, por exemplo:
uma consulta lenta, recuperação em caso de perca ou danificação de dados, controlar os acessos ao banco de dados, monitorar, resolver bloqueios, deadlocks e entre outras funções.

Imagine o seguinte cenário:<br>
SELECT * FROM pedidos WHERE cliente_id = 150000;<br>
E essa consulta demora 8 segundos, o DBA precisa investigar, e descobre que em milhões de pedidos não existe índice em cliente_id.
Então ele pode criar: <br>
CREATE INDEX idx_pedidos_cliente_id ON pedidos (cliente_id);<br>
Depois verifica novamente: <br>
EXPLAIN ANALYZE SELECT * FROM pedidos WHERE cliente_id = 150000;

**Usuários e permissões**

O DBA também deve controlar o acesso ao banco de dados. Por exemplo, se existem os cargos: DBA, desenvolvedor, Sistema, Analista e Relatórios.
Não é correto todos os cargos terem as mesmas liberdades, então o DBA pode criar usuários e permissões:

CREATE USER sistema WITH PASSWORD 'senha_forte'<br>
E conceder apenas o necessário:<br>
GRANT CONNECT ON DATABASE empresa TO sistema;<br>

Por exemplo:

| Usuário | permissões |
|----------|----------|
|DBA | pode administrar o banco|
|Desenvolvedor | pode trabalhar em determinadas tabelas|
|Sistema | pode inserir/consultar dados|
|Relatórios | somente leitura|

Seguindo o princípio de menor privilégio.

**Backup**

O DBA também é responsável pelo Backup de dados. Imagine que uma empresa tenha 10 anos de dados de clientes e vendas, e um dia acontece uma falha no servidor e o banco fica indisponível, o DBA precisa de um estratégia de backup.

Por exemplo:<br>
pg_dump -U postgres empresa > empresa.sql

Mas em uma empresa grande não basta simplesmente fazer um backup manual. Portanto o DBA precisa pensar:

- Com que frequência fazer backup
- Onde armazenar
- Se backup está funcionando
- Se o restore já foi testado
- Quanto tempo é possivel ficar sem o sistema
- Quantos dados podem ser perdidos

Um DBA também precisa estar preparado para casos de recuperação de desastres, como em caso de alguém acidentalmente apagar uma tabela:

DROP TABLE clientes;

O DBA precisa saber:

- Se xiste backup
- Se existe WAL
- Se é possível recuperar
- Qual era o estado do banco antes do problema
- Quanto tempo levará a recuperação

Em ambientes críticos, o objetivo pode ser recuperar o banco para um determinado momento. Isso envolve conceitos como: WAL → backup → recuperação → PITR

**Monitoramento**

O DBA não deve esperar o sistema quebrar para descobrir que existe um problema, então uma de suas funções também é conferir se tudo está de acordo. E ele monitora:

- CPU
- Memória
- Disco
- Conexões
- Locks
- Queries lentas
- WAL
- Replicação
- Tamanho das tabelas
- Índices
- Autovacuum

Por exemplo: <br>
SELECT * FROM pg_stat_activity;<br>
Isso permite verificar as sessões/conexões ativas, como:

|PID |Usuário |Estado|
|-----|--------|------|
|1254 |sistema |active|
|1255 |sistema |active|
|1256 |sistema |idle|
|1257 |sistema |active|

Resolver bloqueios e deadlocks também faz parte do papel de um DBA. Imagine:

Usuário A
: bloqueia tabela X,
espera tabela Y

Usuário B
: bloqueia tabela Y,
espera tabela X

Temos uma situação de deadlock. O DBA precisa investigar as sessões e entende quem está bloqueando quem.
No PostgreSQL, existem ferramentas e mecanismos relacionados a locks, como:
pg_stat_activity

Para manutenção do banco, o PostgreSQL possui mecanismos que são fundamentais para a saúde do banco, como:

- VACUUM
- ANALYZE
- Autovacuum

Quando muitos registros são atualizados ou excluídos, o PostgreSQL precisa realizar manutenção interna. O DBA verifica se o autovacuum está funcionando adequadamente.

Planejar crescimento também é uma função do DBA, imagine:

|Ano| Memória|
|----|------|
|2026| 500 GB|
|2027| 1 TB|
|2028| 2 TB|
|2029| 5 TB|

O DBA precisa pensar antecipadamente se o servidor vai suportar o crescimento. Ele pode precisar planejar:

- armazenamento
- memória
- CPU
- particionamento
- arquivamento
- índices
- replicação
- retenção de dados

Em empresas críticas, o banco não pode simplesmente ficar fora do ar, então é necessário uma alta disponibilidade, é comum o uso de réplicas de um servidos, para caso o principal apresentar algum problema.

Ou seja, o trabalho de um DBA é estruturar e organizar, garantir a performance e segurança, restaurar após falhas, e monitorar o banco de dados.

### 1.2 Perfis de usuários de banco de dados

Em bancos de dados existem diferentes tipos de usuários, que são classificados com base em suas funções e em como vão interagir com o banco.

- **DBA**: o admisnistrador do banco de dados, como já descrito previamente, é responsável por estruturar e organizar, garantir a performance e segurança, restaurar após falhas, e monitorar do banco de dados.

- **Programadores de aplicação**: são os programadores, também conhecidos como analistas de sistemas ou engenheiros de software, eles desenvolvem o código que vai interagir com o banco de dados e ajudar os usuários, usando linguagens como Java, C, Python, etc. 

- **Usuários sofisticados**: interagem com o banco de dados sem criar programas, interagem usando comandos SQL, eles realizam consultas e análises.

- **Usuários especialistas**: Criam aplicações especializadas e sistemas não tradicionais de processamento de dados.

- **Usuários navegantes**: Usuários que acessam o banco de dados buscnado por informações, interagem através de interfaces de fácil acesso e não tem conhecimento sobre SQL e a estrutura interna do banco de dados.

### 1.3 Riscos do uso de IA por usuários especialistas
Com o avanço tecnológico, a IA tem se tornado cada vez mais comum como uma ferramenta prática com diversos benefícios, chegando até mesmo nas grandes empresas proporcionando possibilidades para a manipulação, análise e gerenciamento de banco de dados. Contudo, o uso da IA por “Usuários Especialistas” que interagem diretamente com a manipulação dos dados de um DB, pode apresentar riscos significativos à segurança e integridade do banco de dados, visto que a capacidade de automatizar consultas, identificar vulnerabilidades e processar grandes volumes de dados pode ser perigoso e ser usado para fins mal-intencionados. Sendo assim, é importante compreender os riscos de uso de IA por esses usuários para aplicar boas medidas de segurança aos dados.

**Privacidade dos Dados**  
Uma das razões pelas quais a IA representa um risco à privacidade de dados maior do que os avanços tecnológicos anteriores é o grande volume de informações em jogo.
Grandes modelos de linguagem (LLMs) são os modelos de IA subjacentes para muitas aplicações de IA generativa, como assistentes virtuais e chatbots de IA conversacional. Como o nome indica, esses modelos de linguagem exigem um volume imenso de dados de treinamento.
Porém, os dados que ajudam a treinar LLMs geralmente são obtidos por rastreadores da web que coletam informações de sites. Terabytes ou petabytes de texto, imagens ou vídeos são incluídos rotineiramente como dados de treinamento e, inevitavelmente, alguns desses dados são confidenciais: informações de saúde, dados pessoais de sites de redes sociais, dados financeiros pessoais, dados biométricos usados para reconhecimento facial e muito mais. Com o aumento da quantidade de dados confidenciais sendo coletados, armazenados e transmitidos, são maiores as chances de que pelo menos uma parte seja exposta ou implementada de maneiras que violem os direitos de privacidade.
Trazendo isso para o banco de dados, as informações coletadas pela IA para aprendizado podem ser manipuladas de forma indevida, e compartilhada sem o consentimento do usuário.

**Vazamento de dados e exposição de modelos**  
Os sistemas de IA generativa frequentemente se conectam a bases de conhecimento internas ou conjuntos de dados proprietários. Sem controles de acesso rigorosos e validação de resultados, os modelos podem revelar informações sensíveis em resposta a consultas estruturadas de forma inteligente.
O vazamento de dados pode envolver informações de identificação pessoal, documentos confidenciais, código-fonte ou dados regulamentados. Além disso, os próprios pesos do modelo podem representar propriedade intelectual valiosa.
Sendo assim, um usuário especialista mal-intencionado pode facilmente acessar os dados através de consultas pelo prompt da IA.

**Alucinações e consultas incorretas**  
Os modelos generativos de IA podem produzir resultados plausíveis, mas incorretos. Em um contexto empresarial, recomendações de configuração equivocadas, orientações de conformidade incorretas ou etapas de correção falhas podem gerar consequências operacionais ou de segurança.
Embora a alucinação não seja um ataque direto, ela se torna um risco de segurança quando os resultados da IA ​​influenciam a tomada de decisões ou processos automatizados.

**Ataques de injeção imediata**  
A injeção de prompts ocorre quando um atacante insere instruções maliciosas no texto de entrada para manipular o comportamento do modelo. Por exemplo, um usuário pode instruir o sistema a ignorar diretivas anteriores e revelar instruções ocultas ou informações confidenciais.
Diferentemente da injeção de SQL, esse ataque não explora uma falha de código. Ele explora como o modelo processa a linguagem. Se as salvaguardas e as camadas de validação forem fracas, o modelo pode ceder. A injeção de prompts é atualmente uma das ameaças mais significativas à IA generativa porque atinge diretamente o comportamento do sistema.

Como visto o uso da IA em um banco de dados pode ser prejudicial se não implementada com as devidas normas de segurança, mantendo a integridade dos dados sem vazamentos e manipulações indevidas no código e nos dados em si.

### 1.4 Distribuição segura de dados
A distribuição segura dos dados é fundamental na utilização de banco de dados, garantindo que os dados sejam armazenados, compartilhados e acessados de forma segura é essencial para evitar vazamentos, alterações indevidas, perdas de informações, e acessos não autorizados, mantendo a integridade e confidencialidade dos dados.
Para isso é necessário adotar diferentes medidas de segurança, como: Controle de acesso, definição de permissões, autenticação de usuários e monitoramento.

**Controle de Acesso e Permissões**  
Um número mínimo prático de usuários deve ter acesso ao banco de dados, e suas permissões devem ser restritas aos níveis mínimos necessários para que realizem seus trabalhos. Da mesma forma, o acesso à rede deve ser limitado ao nível mínimo de permissões necessárias.
Os controles de acesso geralmente são baseados em políticas. Os administradores de sistema elaboram políticas de controle de acesso que detalham as permissões dos sujeitos.
As políticas de acesso determinam os objetos que um sujeito pode acessar, APIs que uma aplicação pode chamar, os conjuntos de dados que um grande modelo de linguagem (LLM) pode ingerir. Eles também, de forma crucial, ditam o que os usuários individuais podem fazer com um objeto.

**Autenticação**  
Quando um usuário deseja acessar um recurso protegido por um sistema de controle de acesso, ele primeiro verifica sua identidade por meio de um processo de autenticação.
A autenticação geralmente envolve a apresentação de um conjunto de credenciais, como uma combinação de nome de usuário e senha. No entanto, as senhas são consideradas algumas das credenciais mais fracas porque os agentes da ameaça podem facilmente adivinhá-las ou roubá-las.
Atualmente, a maioria dos sistemas depende de medidas mais substanciais, como biometria e autenticação multifator (MFA). A MFA exige duas ou mais evidências para comprovar a identidade de um usuário, como a digitalização da impressão digital ou uma senha de uso único gerada por um aplicativo de autenticação.

**Funções Personalizadas**  
Com a definição de funções personalizadas (ou roles), é possível configurar e gerenciar quais permissões e quais usuários terão acesso a essas permissões, criando um perfil único com permissões pré estabelecidas que podem ser atribuídas aos usuários, isso evita erros nos acessos e torna fácil de configurar quais tipos de usuários devem acessar determinados dados.

**Visões**  
São tabelas de acesso que ditam quais dados serão acessados em uma consulta ao banco de dados. Além de organizar os dados na apresentação da consulta, também restringe o acesso apenas aos dados necessários da consulta de cada usuário.
Configurar visões permite controlar o que cada usuário pode consultar no bando de dados.

**Auditoria**  
O processo de auditoria é registrar, monitorar e analisar as atividades ocorridas dentro do banco de dados. Informações de acesso, edição e consulta é importante para encontrar falhas e ocorrências indesejadas, garantindo a segurança e privacidade. Esse processo é feito por meio de gravação de logs de registro de ações realizadas no banco de dados e armazenadas em disco.
Por meio da auditoria é possível saber quais usuários acessaram e o que acessaram, alterações dos dados, registros de edição dos privilégios e permissões e alteração nas estruturas do banco de dados.

### 1.5 Atuação do DBA no cenário de IA

No cenário em que usuários especialistas utilizam ferramentas de Inteligência Artificial para gerar consultas SQL, o papel do DBA (Database Administrator) se torna ainda mais importante. A IA pode ajudar na criação de consultas e na análise de dados, mas não deve ser tratada como responsável por decidir quais dados podem ser acessados ou quais alterações devem ser realizadas no banco. O DBA continua responsável pela administração, segurança, integridade e desempenho do banco de dados.

Além das atividades técnicas, o DBA também possui uma função preventiva e estratégica. Isso significa que ele não deve apenas corrigir problemas depois que eles acontecem, mas também criar políticas e controles para diminuir a possibilidade de falhas, acessos indevidos e perda de desempenho.

**Monitoramento**

Uma das responsabilidades do DBA é acompanhar o comportamento das consultas executadas no banco. Isso se torna ainda mais importante quando os usuários utilizam IA, pois uma consulta gerada automaticamente pode estar correta em relação à sintaxe do SQL, mas ainda assim apresentar uma lógica inadequada ou consumir recursos excessivos.

Por exemplo, um usuário pode solicitar à IA um relatório sobre as vendas da empresa e receber uma consulta que percorra uma grande quantidade de registros sem utilizar os índices de maneira adequada. Mesmo que a consulta funcione, ela pode consumir muita CPU, memória e disco e prejudicar o desempenho das demais operações.

O PostgreSQL possui recursos para auxiliar nesse acompanhamento. Um exemplo é a extensão pg_stat_statements, que registra estatísticas relacionadas às consultas executadas e permite identificar comandos que apresentam alto consumo de recursos ou que são executados com muita frequência. Dessa forma, o DBA consegue analisar o comportamento do banco e identificar consultas que precisam ser revisadas. Assim, a IA pode aumentar a produtividade na criação de consultas, mas o DBA deve manter mecanismos de monitoramento para identificar consultas lentas, abusivas ou que não estejam de acordo com as necessidades do sistema.

**Políticas de Acesso**

Outra função importante do DBA é definir quais usuários podem acessar determinados dados e quais operações podem realizar. No PostgreSQL, esse controle pode ser realizado por meio de roles e privilégios, permitindo organizar os usuários e limitar o acesso aos objetos do banco.

No cenário apresentado, um usuário especialista não deve receber acesso irrestrito ao banco apenas porque utiliza uma ferramenta de IA para gerar SQL. O DBA pode criar uma role específica para esse perfil e conceder somente as permissões necessárias para suas atividades.

Por exemplo, um usuário que trabalha com análise de vendas pode ter permissão para consultar informações sobre quantidade de vendas e valores totais, mas não necessariamente precisa acessar CPF, endereço ou outros dados pessoais dos clientes. Essa separação diminui o impacto de uma eventual consulta incorreta ou inadequada gerada pela IA, pois a consulta continuará limitada às permissões que foram concedidas ao usuário.

**Auditoria**

A utilização de IA também aumenta a importância da auditoria. O DBA precisa ter condições de identificar quais operações foram realizadas, por qual usuário e, quando possível, quais dados foram acessados.

Esse acompanhamento é importante, por exemplo, se uma consulta gerada por IA retornar uma quantidade muito maior de dados do que o esperado ou tentar acessar informações que não fazem parte das atividades daquele usuário.

A auditoria também permite investigar incidentes e identificar comportamentos fora do padrão. Dessa maneira, os registros das operações ajudam o DBA a entender o que aconteceu e tomar medidas corretivas. Além disso, recursos de segurança, controle de acesso e auditoria são importantes para proteger informações confidenciais e manter a conformidade com regras de privacidade.

**Orientação aos Usuários**

O DBA também deve orientar os usuários especialistas sobre o uso adequado das ferramentas de IA. Uma resposta produzida por uma IA não deve ser considerada automaticamente correta apenas porque possui uma aparência técnica ou porque a consulta foi executada sem apresentar um erro de sintaxe.

Uma consulta gerada por IA deve ser revisada antes de ser executada, principalmente quando envolve dados pessoais, alterações ou exclusões de registros ou operações que possam consumir muitos recursos. Essa preocupação é importante porque a IA pode sugerir soluções sem compreender completamente o contexto específico da organização. No caso de bancos de dados, uma decisão inadequada pode afetar segurança, desempenho e integridade dos dados. Por isso, a análise humana continua sendo necessária.

Também é importante orientar os usuários a não enviar dados pessoais ou informações confidenciais do banco para ferramentas externas de IA sem autorização. A proteção desses dados deve estar de acordo com as políticas de segurança da organização e com a LGPD.

Nesse sentido, o DBA pode contribuir para a criação de regras que definam quais ferramentas podem ser utilizadas, quais informações podem ser fornecidas à IA e quais procedimentos devem ser realizados antes da execução de uma consulta.

**Performance e Backups**

O DBA também é responsável por manter o desempenho e a disponibilidade do banco. Com o aumento do uso de IA para geração de consultas, pode existir uma quantidade maior de consultas complexas ou pouco eficientes. Por isso, o DBA deve acompanhar o desempenho do sistema e analisar consultas que apresentem consumo elevado de recursos. Quando necessário, pode avaliar índices, estrutura das consultas e outras configurações que contribuam para melhorar o desempenho.

A atuação do DBA também envolve a realização e o gerenciamento de backups e procedimentos de recuperação. A documentação oficial do PostgreSQL apresenta diferentes estratégias para backup e recuperação dos dados, que são fundamentais para reduzir os impactos de falhas ou problemas no ambiente.

No cenário de uso de IA, essa responsabilidade continua sendo importante. Uma consulta ou operação inadequada não deve resultar em uma perda permanente dos dados. Por isso, além de realizar backups, é importante verificar se os procedimentos de recuperação realmente funcionam. 

De maneira geral, o DBA atua como uma camada de controle entre os usuários, as ferramentas de IA e os dados da organização. A IA pode facilitar a criação de consultas e auxiliar em tarefas de análise, mas cabe ao DBA estabelecer limites, monitorar o ambiente e garantir que segurança, integridade, privacidade e desempenho sejam preservados. Essa combinação entre automação e supervisão humana também é apontada em discussões recentes sobre a evolução da função do DBA com a IA.

### 1.6 Análise crítica: qual a melhor abordagem?

Na avaliação e posicionamento do grupo, a melhor abordagem para distribuir dados em um ambiente onde usuários especialistas utilizam Inteligência Artificial é não impedir o uso da IA, mas controlar de forma rigorosa o acesso aos dados e às operações que esses usuários podem realizar.

A IA pode ser uma ferramenta muito útil para auxiliar na criação de consultas e na análise de informações. Porém, permitir que uma ferramenta de IA tenha acesso amplo e irrestrito ao banco representaria um risco significativo, principalmente quando existem dados pessoais e informações importantes para a organização. Por isso, o uso da IA deve acontecer dentro de uma estrutura de segurança definida pelo DBA.

O primeiro princípio dessa abordagem deve ser o menor privilégio (least privilege). Cada usuário deve receber somente as permissões necessárias para realizar suas atividades. No PostgreSQL, as roles permitem organizar usuários e controlar os privilégios concedidos a eles. Além disso, o grupo considera importante utilizar views para disponibilizar somente os dados necessários. Dessa maneira, o usuário pode realizar suas análises sem precisar acessar diretamente todas as colunas das tabelas originais.

Essa estratégia também está relacionada à LGPD, que estabelece princípios como segurança e prevenção e determina a adoção de medidas técnicas e administrativas para proteger os dados pessoais contra acessos não autorizados e situações inadequadas de tratamento. Outro ponto fundamental é a auditoria. Não basta limitar o que cada usuário pode fazer; também é necessário acompanhar as operações realizadas. Dessa forma, se uma consulta gerada por IA apresentar um comportamento inesperado, o DBA poderá identificar a operação, analisar o ocorrido e tomar medidas corretivas.

O controle de desempenho também deve fazer parte dessa estratégia. Consultas geradas automaticamente podem ser complexas e consumir muitos recursos. Por isso, o DBA deve acompanhar o comportamento das consultas e identificar aquelas que possam prejudicar o funcionamento do banco. Recursos do PostgreSQL, como pg_stat_statements, podem ajudar nesse monitoramento. Assim, mesmo que a IA gere uma consulta inadequada, o usuário não terá automaticamente acesso a todas as informações existentes no banco. A combinação entre menor privilégio, views, roles, auditoria e monitoramento reduz os riscos e permite aproveitar os benefícios da IA de maneira mais segura.

Portanto, a IA deve ser vista como uma ferramenta de apoio, e não como uma autoridade sobre os dados. Quanto maior a capacidade de uma ferramenta de IA de gerar consultas e trabalhar com informações, maior também deve ser a preocupação com permissões, auditoria, validação e governança.

Dessa forma, o grupo entende que é possível aproveitar os ganhos de produtividade proporcionados pela IA sem abrir mão da segurança, da integridade, da privacidade e do desempenho do banco de dados.

## 2. Exemplos e Casos

Imagine um sistema de clínica médica, a equipe de recepção deve consultar dados de contato dos pacientes, mas não deve ter acesso aos prontuários médicos sensíveis.

```

//Cria uma tabela do tipo pacientes
CREATE TABLE pacientes (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cpf VARCHAR(14) UNIQUE NOT NULL,
    telefone VARCHAR(20),
    prontuario_medico TEXT,
    ativo BOOLEAN DEFAULT TRUE
);

//Cadastra exemplos de pacientes
INSERT INTO pacientes (nome, cpf, telefone, prontuario_medico, ativo) VALUES
('Ana Silva', '111.222.333-44', '(11) 98765-4321', 'Histórico de hipertensão leve.', TRUE),
('Carlos Souza', '555.666.777-88', '(11) 91234-5678', 'Paciente em acompanhamento dermatológico.', TRUE),
('Marcos Lima', '999.888.777-66', '(11) 97777-8888', 'Cadastro inativado a pedido.', FALSE);

//Cria visão somente para o pacientes ativos e oculta o prontuário
CREATE VIEW clientes_visiveis AS
SELECT 
    id,
    nome,
    telefone,
    ativo
FROM pacientes
WHERE ativo = TRUE;

//Cria o grupo para o pessoal da recepção
CREATE ROLE recepcao;

//Concede o acesso da visão clientes_visiveis para o grupo recepcao
GRANT SELECT ON clientes_visiveis TO recepcao_group;

//Cria o usuário para a atendente maria e coloca ela no respectivo grupo
CREATE USER atendente_maria WITH PASSWORD 'SenhaSegura123!';
GRANT recepcao_group TO atendente_maria;

```

## 3. Referências

GEEKSFORGEEKS. Different Types of Database UsersGeeksforGeeks. [S.l.: S.n.]. Disponível em: <https://www.geeksforgeeks.org/dbms/different-types-of-database-users/>. Acesso em: 15 ago.. 2026.

KOSINSKI, Matthew. Banco de dadosIbm.com. [S.l.: S.n.]. Disponível em: <https://www.ibm.com/br-pt/think/topics/database>. Acesso em: 15 ago.. 2026.

Date, Christopher J. Introdução a sistemas de bancos de dados. Elsevier Brasil, 2004.

POSTGRESQL GLOBAL DEVELOPMENT GROUP. PostgreSQL Documentation — Database Roles. Disponível em: https://www.postgresql.org/docs/current/user-manag.html. Acesso em: 12 ago. 2026.

POSTGRESQL GLOBAL DEVELOPMENT GROUP. PostgreSQL Documentation — pg_stat_statements. Disponível em: https://www.postgresql.org/docs/current/pgstatstatements.html. Acesso em: 12 ago. 2026.

POSTGRESQL GLOBAL DEVELOPMENT GROUP. PostgreSQL Documentation — File System Level Backup. Disponível em: https://www.postgresql.org/docs/current/backup-file.html. Acesso em: 12 ago. 2026.

BRASIL. Autoridade Nacional de Proteção de Dados (ANPD). Glossário ANPD — Medidas de Segurança, Técnicas e Administrativas. Disponível em: https://www.gov.br/anpd/pt-br/documentos-e-publicacoes/glossario-anpd. Acesso em: 12 ago. 2026.

BRASIL. Lei nº 13.709, de 14 de agosto de 2018 — Lei Geral de Proteção de Dados Pessoais (LGPD). 

What is a database administrator (DBA)?. Oracle APAC, 2020. Disponível em: https://www.oracle.com/apac/database/what-is-a-dba/. Acesso em: 15 ago. 2026. 

EDDY, Nathan. Database Administrators and AI: What to Know, How to Grow. Dice, 2023. Disponível em: https://www.dice.com/career-advice/database-administrators-and-ai-what-to-know-how-to-grow. Acesso em: 15 ago. 2026. 

DSA, Equipe . Qual a Importância de Um DBA (Database Administrator) em Projetos de Data Science?. Data Science Academy, 2025. Disponível em: https://blog.dsacademy.com.br/qual-a-importancia-de-um-dba-database-administrator-em-um-projeto-de-data-science/. Acesso em: 15 ago. 2026.

## 4. Conclusões

Aprendizados, reflexões e principais pontos observados pelo grupo.

## Link do Repositório Git
