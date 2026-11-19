# LabQA: Central de Chamados

Projeto da disciplina de Teste de Software. É uma central de chamados de suporte feita em HTML, CSS e JavaScript, testada com Selenium e pytest.

O sistema não tem backend. Os chamados ficam salvos no localStorage do próprio navegador, então não use dados pessoais reais.

## Como rodar

```powershell
py -m pip install -r requirements.txt
py -m http.server 8000
```

Com o servidor aberto, rode os testes em outro terminal:

```powershell
py -m pytest --html=relatorio.html --self-contained-html
```

O arquivo relatorio.html na raiz do projeto tem o resultado da última execução: 22 testes aprovados.

## Arquivos

| Arquivo | Para que serve |
|---|---|
| index.html | Formulário, indicadores, filtros e fila de chamados |
| style.css | Visual da página |
| app.js | Valida, cria, salva, pesquisa, filtra, muda status e exclui chamados |
| tests/conftest.py | Abre o Chrome limpo para cada teste e fecha no final |
| tests/pages.py | Page Object com as ações da tela de chamados |
| tests/test_chamados.py | Os 17 cenários de teste |
| pytest.ini | Configuração do pytest |
| requirements.txt | Bibliotecas necessárias |

## Requisitos do sistema

| ID | Requisito | Critério de aceitação |
|---|---|---|
| RF-01 | Abrir chamado | Só dados válidos criam registro e mostram confirmação |
| RF-02 | Validar campos | Título com 5 ou mais caracteres, solicitante com 3 ou mais e descrição com 10 ou mais. Categoria e prioridade obrigatórias |
| RF-03 | Status inicial | Todo chamado começa como Aberto |
| RF-04 | Pesquisar | Busca parcial por título ou solicitante, sem diferenciar maiúsculas |
| RF-05 | Filtrar status | Todos, Aberto, Em andamento e Concluído |
| RF-06 | Mudar status | O status pode ser alterado e fica salvo |
| RF-07 | Excluir | Remove o chamado e atualiza os indicadores |
| RF-08 | Persistir | Os chamados continuam lá depois de atualizar a página |
| RF-09 | Proteger a tela | Texto digitado aparece como texto, nunca como HTML executável |

## Cenários de teste

| ID | Cenário | Resultado esperado |
|---|---|---|
| CT-01 | Abertura com campos válidos | Cria o chamado e o total fica 1 |
| CT-02 | Campos vazios | Mostra as mensagens de validação |
| CT-03 | Título curto com 3 entradas | Bloqueia cada título inválido |
| CT-04 | Pesquisa por título | Encontra o chamado ou mostra a lista vazia |
| CT-05 | Pesquisa por solicitante | Encontra o solicitante |
| CT-06 | Mudança de status | Atualiza o status e os indicadores |
| CT-07 | Filtro por status | Mostra só o status escolhido |
| CT-08 | Excluir chamado | O total volta a zero |
| CT-09 | Persistência após F5 | O chamado continua registrado |
| CT-10 | Limpar formulário | O campo preenchido fica vazio |
| CT-11 | Texto com tags HTML | Mostra o texto sem criar script na página |
| CT-12 | Descrição curta | Mostra erro de tamanho mínimo |
| CT-13 | Título inválido com 3 novas entradas | Bloqueia e o total continua 0 |
| CT-14 | Título no limite com 5 e 6 caracteres | Aceita o chamado |
| CT-15 | Busca parcial em maiúsculas | Encontra o chamado mesmo com letras diferentes |
| CT-16 | Status depois de F5 | O status Concluído continua salvo |
| CT-17 | Descrição com 9 caracteres | Bloqueia e o total continua 0 |

São 17 funções de teste. Como o CT-03 e o CT-13 rodam 3 vezes cada e o CT-14 roda 2 vezes, o pytest coleta 22 execuções no total.

## Valores limite usados

O app tira os espaços do começo e do fim antes de contar os caracteres. Por isso " ABCD " conta como 4 e é bloqueado.

| Campo | Entrada | Esperado |
|---|---|---|
| Título | "ABCD" (4) | Bloquear |
| Título | "ABCDE" (5) | Aceitar |
| Título | "ABCDEF" (6) | Aceitar |
| Título | " ABCD " (4 sem os espaços) | Bloquear |
| Solicitante | "Lia" (3) | Aceitar |
| Descrição | "123456789" (9) | Bloquear |
| Descrição | "1234567890" (10) | Aceitar |
