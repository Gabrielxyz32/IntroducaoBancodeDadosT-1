
# Atividade Teórica: Usuários Especialistas, IA e Distribuição Segura de Dados

**Aluno(s):** Nome1, Nome2, Nome3
**Turma:** Banco de Dados 2026
**Data:** ../../2026
**Repositório Git:** 

## Resumo Executivo

## 1. Desenvolvimento Teórico

### 1.1 O que é o DBA e quais suas funções?

### 1.2 Perfis de usuários de banco de dados

### 1.3 Riscos do uso de IA por usuários especialistas

### 1.4 Distribuição segura de dados

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

## 3. Referências

POSTGRESQL GLOBAL DEVELOPMENT GROUP. PostgreSQL Documentation — Database Roles. Disponível em: https://www.postgresql.org/docs/current/user-manag.html. Acesso em: 12 ago. 2026.

POSTGRESQL GLOBAL DEVELOPMENT GROUP. PostgreSQL Documentation — pg_stat_statements. Disponível em: https://www.postgresql.org/docs/current/pgstatstatements.html. Acesso em: 12 ago. 2026.

POSTGRESQL GLOBAL DEVELOPMENT GROUP. PostgreSQL Documentation — File System Level Backup. Disponível em: https://www.postgresql.org/docs/current/backup-file.html. Acesso em: 12 ago. 2026.

BRASIL. Autoridade Nacional de Proteção de Dados (ANPD). Glossário ANPD — Medidas de Segurança, Técnicas e Administrativas. Disponível em: https://www.gov.br/anpd/pt-br/documentos-e-publicacoes/glossario-anpd. Acesso em: 12 ago. 2026.

BRASIL. Lei nº 13.709, de 14 de agosto de 2018 — Lei Geral de Proteção de Dados Pessoais (LGPD). 

What is a database administrator (DBA)?. Oracle APAC, 2020. Disponível em: https://www.oracle.com/apac/database/what-is-a-dba/. Acesso em: 15 ago. 2026. 

EDDY, Nathan. Database Administrators and AI: What to Know, How to Grow. Dice, 2023. Disponível em: https://www.dice.com/career-advice/database-administrators-and-ai-what-to-know-how-to-grow. Acesso em: 15 ago. 2026. 

DSA, Equipe . Qual a Importância de Um DBA (Database Administrator) em Projetos de Data Science?. Data Science Academy, 2025. Disponível em: https://blog.dsacademy.com.br/qual-a-importancia-de-um-dba-database-administrator-em-um-projeto-de-data-science/. Acesso em: 15 ago. 2026. 

## 4. Conclusões

## Link do Repositório Git
