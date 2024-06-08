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
     - Inner Join: Retorna registros que têm correspondência em ambas as tabelas.
     - Left Join (ou Left Outer Join): Retorna todos os registros da tabela esquerda e os registros correspondentes da tabela direita. Se não houver correspondência, os resultados da tabela direita terão valores NULL.
     - Right Join (ou Right Outer Join): Retorna todos os registros da tabela direita e os registros correspondentes da tabela esquerda. Se não houver correspondência, os resultados da tabela esquerda terão valores NULL.
     - Full Join (ou Full Outer Join): Retorna registros quando há uma correspondência em uma das tabelas. Se não houver correspondência, ainda assim, o resultado aparecerá com NULL nos campos da tabela sem correspondência.
* Having
        - Use HAVING quando precisar filtrar grupos de resultados após a aplicação de funções de agregação. 
        - Use WHERE para filtrar registros antes da agregação.

## Aula 04 - Windows Function
https://www.postgresql.org/docs/current/functions-window.html
* Comparação: GROUP BY vs. Window Functions
    - GROUP BY
        - Agrupa os dados e retorna uma linha por grupo.
        - Utilizado para agregar dados em grupos distintos.
            SELECT 
                product_id, 
                COUNT(*) AS num_sales, 
                SUM(amount) AS total_sales
            FROM sales
            GROUP BY product_id;

            product_id | num_sales | total_sales
            -----------|-----------|------------
            1          | 10        | 1000
            2          | 5         | 500

    - Window Functions
        - Não agrupa os dados, mas realiza cálculos baseados em uma "janela" de linhas relacionada à linha atual.
        - Útil para cálculos que precisam manter os dados originais intactos.
            SELECT 
            product_id, 
            order_date, 
            amount,
            COUNT(*) OVER (PARTITION BY product_id) AS num_sales_per_product,
            SUM(amount) OVER (PARTITION BY product_id) AS total_sales_per_product
            FROM sales;
            product_id | order_date | amount | num_sales_per_product | total_sales_per_product
            -----------|------------|--------|-----------------------|------------------------
            1          | 2024-01-01 | 100    | 10                    | 1000
            1          | 2024-01-02 | 200    | 10                    | 1000
            ...        | ...        | ...    | ...                   | ...
            2          | 2024-01-01 | 300    | 5                     | 500
            ...        | ...        | ...    | ...                   | ...
    - Conclusão
        - Use GROUP BY quando precisar agrupar dados e obter um resultado agregado por grupo.
        - Use funções de janela quando precisar realizar cálculos agregados, mas ainda desejar manter os dados originais em cada linha do resultado.

* RANK(), DENSE_RANK() e ROW_NUMBER()
    - RANK(): Atribui um rank único a cada linha, deixando lacunas em caso de empates.
        SELECT 
        product_id, 
        amount, 
        RANK() OVER (ORDER BY amount DESC) AS rank
        FROM sales;

        product_id | amount | rank
        -----------|--------|-----
        1          | 1000   | 1
        2          | 1000   | 1
        3          | 900    | 3
        4          | 850    | 4

    - DENSE_RANK(): Atribui um rank único a cada linha, com ranks contínuos para linhas empatadas.
        SELECT 
        product_id, 
        amount, 
        DENSE_RANK() OVER (ORDER BY amount DESC) AS dense_rank
        FROM sales;

        product_id | amount | rank
        -----------|--------|-----
        1          | 1000   | 1
        2          | 1000   | 1
        3          | 900    | 2
        4          | 850    | 3

    - ROW_NUMBER(): Atribui um número inteiro sequencial único a cada linha, independentemente de empates, sem lacunas.
        SELECT 
        product_id, 
        amount, 
        ROW_NUMBER() OVER (ORDER BY amount DESC) AS row_number
        FROM sales;

        product_id | amount | rank
        -----------|--------|-----
        1          | 1000   | 1
        2          | 1000   | 2
        3          | 900    | 3
        4          | 850    | 4

* Funções PERCENT_RANK() e CUME_DIST()
    - Ambos retornam um valor entre 0 e 1
            - PERCENT_RANK(): Calcula o rank relativo de uma linha específica dentro do conjunto de resultados como uma porcentagem. É computado usando a seguinte fórmula:
                - RANK é o rank da linha dentro do conjunto de resultados.
                - N é o número total de linhas no conjunto de resultados.
                -  PERCENT_RANK = (RANK - 1) / (N - 1)
            - CUME_DIST(): Calcula a distribuição acumulada de um valor no conjunto de resultados. Representa a proporção de linhas que são menores ou iguais à linha atual. A fórmula é a seguinte:
                - CUME_DIST = (Número de linhas com valores <= linha atual) / (Número total de linhas) 

* Função NTITLE
    -   A função NTILE() no SQL é usada para dividir o conjunto de resultados em um número especificado de partes aproximadamente iguais ou "faixas" e atribuir um número de grupo ou "bucket" a cada linha com base em sua posição dentro do conjunto de resultados ordenado.
