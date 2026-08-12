# Relatorio de validacao tecnica

Data da revisao: 11/08/2026.

## Escopo verificado

- 41 arquivos-fonte Java analisados;
- todos os arquivos passaram por parser de sintaxe Java;
- todos os tipos, imports e chamadas entre classes foram compilados sem erro;
- Models, DAOs, Controllers e Views correspondem aos pacotes da especificacao;
- ausencia de marcadores de pendencia e metodos simulados;
- ausencia de lambda, Stream API, `try-with-resources`, `java.time`, `var`, `record`, diamond operator e demais recursos posteriores ao Java 6;
- todas as tabelas Swing usam modelo somente para leitura;
- inicializacao da interface na Event Dispatch Thread;
- credenciais iniciais tiveram seus hashes recalculados e comparados com o SQL;
- transacoes de emprestimo e devolucao possuem `setAutoCommit(false)`, `commit` e `rollback`;
- script SQL revisado quanto a nomes de colunas, PKs, FKs, indices e dados iniciais.

## Resultado de compilacao

Compilacao concluida sem erros em um compilador OpenJDK 17 usando a API de compatibilidade `--release 7`, menor release oferecida por esse compilador. Em seguida, o fonte foi auditado especificamente para remover construcoes posteriores ao Java 6. O projeto Eclipse fixa `source`, `compliance` e `target` em `1.6`.

Os avisos restantes no compilador moderno sao esperados: as classes Swing como `JComboBox` e `JList` eram usadas sem parametros genericos no Java 6, e `JList.getSelectedValues()` foi descontinuado somente em versoes posteriores.

## Limite do ambiente de validacao

O ambiente de construcao nao disponibilizou um servidor MySQL nem o Connector/J 5.1.49. Por isso, os testes de integracao com banco e os testes visuais devem ser executados no computador de destino depois de configurar `Conexao.java` e adicionar o JAR. Os cenarios completos estao em `Roteiro_Testes.md`.

## Dependencia externa

O projeto nao redistribui o binario do MySQL Connector/J. O arquivo `lib/LEIA-ME.txt` e o `README.md` indicam a versao 5.1.49 e o procedimento para adiciona-la ao Eclipse.
