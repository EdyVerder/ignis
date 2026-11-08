<p align="center"><img src="assets/capa.jpg" width="880" alt="Ignis: integração de dados de ponta a ponta, direto no navegador"></p>

<h1 align="center">Ignis</h1>

<p align="center">
  <b>Plataforma de ETL na web: desenhe, execute, agende e acompanhe integrações de dados direto no navegador.</b><br>
  Nasceu do dia a dia com o Pentaho PDI, se inspirou no Apache Hop e quer ir além dos dois.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/status-em%20produção%20interna-1a7f37?style=flat-square" alt="em produção interna">
  <img src="https://img.shields.io/badge/python-3.12-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python 3.12">
  <img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" alt="FastAPI">
  <img src="https://img.shields.io/badge/SQL%20Server-CC2927?style=flat-square&logo=microsoftsqlserver&logoColor=white" alt="SQL Server">
  <img src="https://img.shields.io/badge/próximo%20passo-App%20%2B%20Web-ff8a5b?style=flat-square" alt="App + Web">
</p>

---

<p align="center"><img src="assets/ignis.gif" width="880" alt="Designer do Ignis: passos ligados em fluxo, SQL com destaque, execução com parâmetros, métricas em cada passo e prévia dos dados"></p>

## Por que existe

O Pentaho Data Integration sempre esteve no centro das cargas de dados, e o Apache Hop mostrou como esse modelo pode
evoluir. O Ignis mantém o que os dois fazem muito bem, que é **desenhar transformações e jobs visualmente, passo a
passo**, e resolve o que fazia falta no dia a dia:

- abrir pelo navegador, sem instalar nada na máquina de quem usa;
- ver **o que rodou, o que falhou e por quê** sem caçar log em servidor;
- agendar sem depender de script solto no Agendador do Windows;
- segurança desde o primeiro dia: usuário da rede, papéis e conexões somente leitura por padrão;
- migrar aos poucos: o Ignis **lê os fluxos do Pentaho** (`.ktr` e `.kjb`) e roda lado a lado.

## O que já faz

| | |
|---|---|
| **Designer visual** | transformações e jobs no navegador, com mais de 50 tipos de passo (entrada de tabela, calculadora, filtro, fórmula, junções, busca em banco, dimensões, REST e GraphQL, Excel, JSON, script Python...) |
| **Execução com métricas** | linhas lidas, gravadas, rejeitadas e tempo em cada passo, em tempo real |
| **Prévia** | os dados de qualquer ponto do fluxo, sem gravar nada |
| **SQL** | editor com destaque de sintaxe e variáveis de execução (`${DATA_INICIO}`) |
| **Jobs** | orquestração com caminho de sucesso e de falha |
| **Histórico** | cada execução com métricas, log e o **erro explicado em linguagem clara** |
| **Agendamentos** | integrados, descritos em linguagem simples ("todo dia às 02:00") |
| **Explorador de bancos** | tabelas, colunas, prévia e "criar transformação" em um clique |
| **Repositório** | pastas, busca e histórico de versões de cada objeto |
| **Importação do Pentaho** | lê `.ktr` e `.kjb` e converte os passos conhecidos |

## Telas

<table>
  <tr>
    <td width="50%"><img src="assets/painel.jpg" alt="Painel no tema escuro: últimas execuções, gráfico dos últimos 14 dias e próximas execuções agendadas"><br><sub><b>Painel</b> · o que rodou, o que falhou e o que vem a seguir</sub></td>
    <td width="50%"><img src="assets/designer.jpg" alt="Designer com as métricas de cada passo depois da execução"><br><sub><b>Designer</b> · métricas em cada passo: 6.200 linhas lidas, 5.321 gravadas</sub></td>
  </tr>
  <tr>
    <td><img src="assets/sql.jpg" alt="Editor de SQL do passo de entrada com destaque de sintaxe e variável de execução"><br><sub><b>SQL</b> · destaque de sintaxe e variáveis de execução</sub></td>
    <td><img src="assets/parametros.jpg" alt="Janela de parâmetros antes de executar a transformação"><br><sub><b>Execução</b> · parâmetros informados na hora</sub></td>
  </tr>
  <tr>
    <td><img src="assets/previa.jpg" alt="Prévia dos dados no meio do fluxo"><br><sub><b>Prévia</b> · dados de qualquer ponto do fluxo</sub></td>
    <td><img src="assets/job.jpg" alt="Job com caminhos de sucesso e de falha"><br><sub><b>Jobs</b> · caminho de sucesso e de falha</sub></td>
  </tr>
  <tr>
    <td><img src="assets/historico.jpg" alt="Histórico de execuções com o erro de uma carga explicado"><br><sub><b>Histórico</b> · métricas, log e o erro explicado</sub></td>
    <td><img src="assets/bancos.jpg" alt="Explorador de bancos com colunas, tipos e prévia de uma tabela"><br><sub><b>Explorador de bancos</b> · colunas, prévia e transformação em um clique</sub></td>
  </tr>
</table>

## Arquitetura

```mermaid
flowchart LR
    U[Navegador] -->|HTTPS| W[FastAPI + Uvicorn<br/>telas Jinja e JavaScript]
    W --> R[(Repositório<br/>transformações, jobs,<br/>versões, histórico)]
    W --> M[Motor de execução<br/>passos em fluxo, métricas]
    S[Agendador interno] --> M
    M --> B[(Bancos<br/>SQL Server, MySQL, SQLite)]
    M --> A[APIs REST e GraphQL]
    M --> X[Arquivos<br/>Excel, CSV, JSON]
    P[Fluxos do Pentaho<br/>.ktr / .kjb] -->|importação| R
```

## Segurança

- entrada com o **usuário da rede** (LDAPS) e permissões por papel;
- **somente HTTPS** e acesso restrito à rede interna;
- conexões novas nascem **somente leitura**; gravar exige liberação explícita;
- senhas de conexão **cifradas** no repositório e nunca exibidas na tela;
- cada execução registra quem disparou, quando e o que aconteceu.

## Próximos passos

Hoje o Ignis roda **na web**. O próximo passo é a versão **aplicativo**, usando o **mesmo repositório**, para quem
prefere trabalhar no desktop sem abrir mão da web.

## Sobre o código

O Ignis está em produção interna, com dados reais, e por isso **o código-fonte não é público**. Tudo o que aparece
nesta página foi gravado numa cópia isolada, com bancos e cadastros **fictícios**.

As técnicas que ele usa aparecem em projetos abertos deste perfil:
[espelho incremental para o DW](https://github.com/EdyVerder/dw-tempo-real),
[conferência por regras](https://github.com/EdyVerder/conferencia-nfse) e
[relatórios automáticos](https://github.com/EdyVerder/relatorios-automaticos).

---

<p align="center"><sub>Feito por <a href="https://github.com/EdyVerder">Rocha</a> · demonstração com dados fictícios</sub></p>
