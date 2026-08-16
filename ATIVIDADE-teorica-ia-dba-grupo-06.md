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


### 1.4 Distribuição segura de dados


### 1.5 Atuação do DBA no cenário de IA


### 1.6 Análise crítica: qual a melhor abordagem?


## 2. Exemplos e Casos

Exemplo de view `clientes_visiveis` no PostgreSQL e exemplo de role/permissão.
Um caso real: sistema de vendas, clínica ou biblioteca.

## 3. Referências

GEEKSFORGEEKS. Different Types of Database UsersGeeksforGeeks. [S.l.: S.n.]. Disponível em: <https://www.geeksforgeeks.org/dbms/different-types-of-database-users/>. Acesso em: 15 ago.. 2026.

KOSINSKI, Matthew. Banco de dadosIbm.com. [S.l.: S.n.]. Disponível em: <https://www.ibm.com/br-pt/think/topics/database>. Acesso em: 15 ago.. 2026.

Date, Christopher J. Introdução a sistemas de bancos de dados. Elsevier Brasil, 2004. 

## 4. Conclusões

Aprendizados, reflexões e principais pontos observados pelo grupo.

## Link do Repositório Git
