# bootcamp_sql_analytics

## Aula 1 - Visão Geral e Preparação do ambiente SQL
* Configuração do ambiente
* Criação banco de dados Norwthind
* Exemplo de Seleção Completa
* Seleção de Colunas Específicas
* Utilizando DISTINCT
* Cláusula WHERE
* ORDER BY
* Utilizando LIKE e IN

## Aula 2 - SQL para Analytics: Nossas primeiras consultas
* DDL (Data Definition Language)
    - Responsável: Administradores de banco de dados (DBAs) e desenvolvedores de banco de dados.
    - CREATE, ALTER, DROP, TRUNCATE

* DML (Data Manipulation Language)
    - Responsável: Desenvolvedores de software, analistas de dados e, ocasionalmente, usuários finais através de interfaces que executam comandos DML por trás dos panos.
    - INSERT, UPDATE, DELETE, MERGE, 

* DQL (Data Query Language)
    - Responsável: Analistas de dados, cientistas de dados, e qualquer usuário que necessite extrair informações do banco de dados.
    - Objetivo: O DQL é usado para consultar e recuperar dados. É fundamental para gerar relatórios, realizar análises, e fornecer dados que ajudem na tomada de decisões. O comando SELECT, parte do DQL, é um dos mais usados e é essencial para qualquer tarefa que requer visualização ou análise de dados.

* DCL (Data Control Language)
    - Responsável: Administradores de banco de dados.
    - GRANT, REVOKE

* TCL (Transaction Control Language)
    - Responsável: Desenvolvedores de software e administradores de banco de dados.
    - COMMIT, ROLLBACK, SAVEPOINT

* ENTENDENDO MELHOR DQL
    - WHERE: Para filtrar registros.
    - GROUP BY: Para agrupar registros.
    - HAVING: Para filtrar grupos.
    - ORDER BY: Para ordenar os resultados.
    - OPERADORES
        - Operador < (Menor que)
        - Operador > (Maior que)
        - Operador <= (Menor ou igual a)
        - Operador >= (Maior ou igual a)
        - Operador <> (Diferente de)
        - Podemos combinar mais de 1 operador;
        - LIKE OR NOT LIKE
        - IN OR NOT IN
    - Funções Agregadas:
        - COUNT, MAX, MIN, SUM, AVG
        - Práticas Recomendadas
            - Precisão de dados: Ao usar AVG() e SUM(), esteja ciente do tipo de dados da coluna para evitar imprecisões, especialmente com dados flutuantes.
            -   NULLs: Lembre-se de que a maioria das funções agregadas ignora valores NULL, exceto COUNT(*), que conta todas as linhas, incluindo aquelas com valores NULL.
            - Performance: Em tabelas muito grandes, operações agregadas podem ser custosas em termos de desempenho. Considere usar índices adequados ou realizar pré-agregações quando aplicável.
            - Clareza: Ao usar GROUP BY, assegure-se de que todas as colunas não agregadas na sua cláusula SELECT estejam incluídas na cláusula GROUP BY.

## Aula 03 - SQL para Analytics: Join and Having in SQL
    * Introdução aos Joins em SQL
        * Inner Join: Retorna registros que têm correspondência em ambas as tabelas.
        *Left Join (ou Left Outer Join): Retorna todos os registros da tabela esquerda e os registros correspondentes da tabela direita. Se não houver correspondência, os resultados da tabela direita terão valores NULL.
        * Right Join (ou Right Outer Join): Retorna todos os registros da tabela direita e os registros correspondentes da tabela esquerda. Se não houver correspondência, os resultados da tabela esquerda terão valores NULL.
        * Full Join (ou Full Outer Join): Retorna registros quando há uma correspondência em uma das tabelas. Se não houver correspondência, ainda assim, o resultado aparecerá com NULL nos campos da tabela sem correspondência.
    * Having
        * Use HAVING quando precisar filtrar grupos de resultados após a aplicação de funções de agregação. 
        * Use WHERE para filtrar registros antes da agregação.
        


