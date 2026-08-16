# Atividade Teórica: Usuários Especialistas, IA e Distribuição Segura de Dados

**Aluno(s):** Gabriel, Geovana, Luan, Murilo, Rebeca
**Turma:** Banco de Dados 2026
**Data:** 14/08/2026
**Repositório Git:** https://github.com/Gabrielxyz32/IntroducaoBancodeDadosT-1

## Resumo Executivo

Breve descrição do tema e da posição adotada pelo grupo.

## 1. Desenvolvimento Teórico

### 1.1 O que é o DBA e quais suas funções?


### 1.2 Perfis de usuários de banco de dados


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


### 1.6 Análise crítica: qual a melhor abordagem?


## 2. Exemplos e Casos

Exemplo de view `clientes_visiveis` no PostgreSQL e exemplo de role/permissão.
Um caso real: sistema de vendas, clínica ou biblioteca.

## 3. Referências

Fontes consultadas (livros, artigos, documentação oficial do PostgreSQL,
materiais do curso).

## 4. Conclusões

Aprendizados, reflexões e principais pontos observados pelo grupo.

## Link do Repositório Git
