# Roteiro de testes manuais

Preencha **Resultado obtido** com `APROVADO`, `REPROVADO` ou uma descricao do comportamento observado.

## T01 - Login valido

- **Objetivo:** confirmar o primeiro acesso.
- **Pre-condicao:** banco criado e usuario admin ativo.
- **Dados:** login `admin`; senha `admin123`.
- **Passos:** iniciar `Main`; informar credenciais; clicar em Entrar.
- **Resultado esperado:** abrir a tela principal, mostrando Administrador e perfil ADMIN no rodape.
- **Resultado obtido:** ______________________________

## T02 - Login invalido

- **Objetivo:** impedir acesso com senha incorreta.
- **Pre-condicao:** aplicacao na tela de login.
- **Dados:** login `admin`; senha `errada`.
- **Passos:** informar os dados; clicar em Entrar.
- **Resultado esperado:** exibir mensagem de usuario ou senha invalidos e permanecer no login.
- **Resultado obtido:** ______________________________

## T03 - Usuario inativo

- **Objetivo:** impedir login de conta inativa.
- **Pre-condicao:** entrar como admin; cadastrar um usuario; depois inativa-lo.
- **Dados:** login `teste.inativo`; uma senha valida.
- **Passos:** sair do sistema; tentar entrar com a conta inativa.
- **Resultado esperado:** acesso recusado.
- **Resultado obtido:** ______________________________

## T04 - Ciclo do cliente

- **Objetivo:** testar cadastro, pesquisa, edicao e inativacao.
- **Pre-condicao:** usuario autenticado.
- **Dados:** nome `Leitor Teste`; CPF `999.999.999-99`; e-mail `leitor@teste.com`.
- **Passos:** abrir Clientes; salvar; pesquisar por `Leitor`; selecionar; editar telefone; salvar; selecionar; inativar.
- **Resultado esperado:** cada mudanca aparece na JTable; registro final permanece com Ativo igual a Nao.
- **Resultado obtido:** ______________________________

## T05 - CPF duplicado

- **Objetivo:** confirmar a restricao de unicidade.
- **Pre-condicao:** cliente de CPF `111.111.111-11` existente.
- **Dados:** novo nome e o mesmo CPF.
- **Passos:** tentar salvar o novo cliente.
- **Resultado esperado:** operacao recusada pelo banco e mensagem exibida, sem duplicacao.
- **Resultado obtido:** ______________________________

## T06 - Cadastros auxiliares

- **Objetivo:** testar Autor, Editora e Categoria.
- **Pre-condicao:** usuario autenticado.
- **Dados:** Autor `Autor Teste`; Editora `Editora Teste`; Categoria `Categoria Teste`.
- **Passos:** em cada aba, cadastrar, pesquisar, editar e excluir o registro sem relacionamentos.
- **Resultado esperado:** tabelas atualizadas depois de cada operacao.
- **Resultado obtido:** ______________________________

## T07 - Livro com varios autores

- **Objetivo:** validar a relacao N:N.
- **Pre-condicao:** ao menos dois autores, uma editora e uma categoria cadastrados.
- **Dados:** titulo `Livro N para N`; ISBN `TESTE-NN-001`; ano `2026`; dois autores.
- **Passos:** cadastrar o livro com Ctrl+clique em dois autores; selecionar o livro na tabela; fechar e reabrir a aba.
- **Resultado esperado:** os dois autores aparecem na tabela e voltam selecionados na edicao.
- **Resultado obtido:** ______________________________

## T08 - Cadastro de exemplar

- **Objetivo:** diferenciar obra de unidade fisica.
- **Pre-condicao:** livro do T07 existente.
- **Dados:** codigo `EXT-NN-001`; status DISPONIVEL.
- **Passos:** abrir Exemplares; selecionar o livro; salvar.
- **Resultado esperado:** nova unidade exibida com livro e status corretos.
- **Resultado obtido:** ______________________________

## T09 - Emprestimo valido

- **Objetivo:** registrar a retirada atomica.
- **Pre-condicao:** cliente ativo e exemplar disponivel.
- **Dados:** hoje como data; hoje + 7 dias como previsao.
- **Passos:** abrir Emprestimos; selecionar cliente, livro e exemplar; registrar.
- **Resultado esperado:** emprestimo ABERTO criado; exemplar desaparece da lista de disponiveis e fica EMPRESTADO em Exemplares.
- **Resultado obtido:** ______________________________

## T10 - Segundo emprestimo do mesmo exemplar

- **Objetivo:** impedir dois emprestimos abertos.
- **Pre-condicao:** emprestimo do T09 aberto.
- **Dados:** mesmo exemplar.
- **Passos:** tentar utilizar o mesmo exemplar novamente, inclusive apos atualizar os cadastros.
- **Resultado esperado:** exemplar nao aparece como disponivel; uma tentativa concorrente e recusada pelo DAO.
- **Resultado obtido:** ______________________________

## T11 - Devolucao valida

- **Objetivo:** devolver e liberar na mesma transacao.
- **Pre-condicao:** emprestimo do T09 aberto.
- **Dados:** data atual.
- **Passos:** abrir Devolucoes; selecionar o registro; confirmar.
- **Resultado esperado:** emprestimo passa a DEVOLVIDO e exemplar volta a DISPONIVEL.
- **Resultado obtido:** ______________________________

## T12 - Devolucao repetida

- **Objetivo:** impedir uma segunda devolucao.
- **Pre-condicao:** T11 concluido.
- **Dados:** mesmo ID de emprestimo.
- **Passos:** atualizar a lista de devolucoes e procurar o registro; opcionalmente chamar novamente o DAO em depuracao.
- **Resultado esperado:** registro nao aparece entre abertos e o DAO recusa nova devolucao.
- **Resultado obtido:** ______________________________

## T13 - Persistencia

- **Objetivo:** confirmar que os dados nao ficam apenas na memoria.
- **Pre-condicao:** cadastros e movimentacoes realizados.
- **Dados:** registros dos testes anteriores.
- **Passos:** fechar a aplicacao; abri-la; entrar novamente; consultar os registros.
- **Resultado esperado:** dados permanecem no MySQL e voltam a aparecer nas tabelas.
- **Resultado obtido:** ______________________________

## T14 - Abas sem duplicacao

- **Objetivo:** validar `abrirAba`.
- **Pre-condicao:** tela principal aberta.
- **Dados:** modulo Clientes.
- **Passos:** clicar varias vezes em Cadastros > Clientes e em Consultas > Clientes.
- **Resultado esperado:** existir somente uma aba Clientes, que apenas recebe foco.
- **Resultado obtido:** ______________________________

## T15 - Validacoes obrigatorias

- **Objetivo:** conferir mensagens antes do DAO.
- **Pre-condicao:** telas de cadastro acessiveis.
- **Dados:** nome vazio, e-mail invalido, ISBN vazio, emprestimo sem exemplar e previsao anterior.
- **Passos:** testar cada combinacao.
- **Resultado esperado:** mensagem clara; nenhum dado parcial gravado.
- **Resultado obtido:** ______________________________

## T16 - Permissao do atendente

- **Objetivo:** restringir a administracao de usuarios.
- **Pre-condicao:** banco inicial.
- **Dados:** login `atendente`; senha `senha123`.
- **Passos:** entrar como atendente; abrir o menu Cadastros.
- **Resultado esperado:** item Usuarios desabilitado; demais operacoes disponiveis.
- **Resultado obtido:** ______________________________

## T17 - Exclusao protegida por relacionamento

- **Objetivo:** preservar integridade referencial.
- **Pre-condicao:** autor, editora ou categoria associados a um livro.
- **Dados:** um cadastro usado pelos livros iniciais.
- **Passos:** tentar exclui-lo.
- **Resultado esperado:** MySQL recusa a exclusao e a aplicacao exibe o erro sem fechar.
- **Resultado obtido:** ______________________________

## T18 - Atualizacao de atraso

- **Objetivo:** marcar emprestimo vencido.
- **Pre-condicao:** criar no banco ou pela tela um emprestimo cuja previsao ja passou.
- **Dados:** status ABERTO; previsao anterior a hoje.
- **Passos:** abrir Emprestimos ou Devolucoes.
- **Resultado esperado:** status apresentado como ATRASADO e devolucao continua permitida.
- **Resultado obtido:** ______________________________
