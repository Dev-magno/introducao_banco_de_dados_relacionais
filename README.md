# COMANDOS SQL

## SQL Básico

| Comando           | Descrição                                                 |
|-------------------|-----------------------------------------------------------|
| SELECT            | Seleciona dados de uma ou mais tabelas                    |
| FROM              | Especifica de qual tabela ou tabelas extrair os dados    |
| WHERE             | Filtra registros com base em uma condição                  |
| INSERT INTO       | Insere novos registros em uma tabela                      |
| UPDATE            | Atualiza registros em uma tabela                           |
| DELETE FROM       | Exclui registros de uma tabela                            |
| DISTINCT          | Retorna valores únicos de uma coluna                      |
| ORDER BY          | Classifica os resultados em ordem ascendente ou descendente|
| GROUP BY          | Agrupa registros com base em valores semelhantes          |
| JOIN              | Combina colunas de duas ou mais tabelas                   |
| INNER JOIN        | Retorna registros quando há pelo menos uma correspondência em ambas as tabelas|
| LEFT JOIN         | Retorna todos os registros da tabela da esquerda e os registros correspondentes da tabela da direita |
| RIGHT JOIN        | Retorna todos os registros da tabela da direita e os registros correspondentes da tabela da esquerda|
| OUTER JOIN        | Retorna registros quando há uma correspondência em qualquer uma das tabelas |

### Alguns Exemplos práticos 
1 - SELECT FROM: Seleciona todos os produtos da tabela Produtos.

```
SELECT * FROM Produtos;
```
2 - INSERT INTO: Insere um novo produto na tabela Produtos.
```
SELECT * FROM Produtos WHERE preco > 50;

```
3 - UPDATE: Atualiza o preço de todos os produtos com a categoria 'Eletrônicos'.
```
UPDATE Produtos SET preco = preco * 0.9 WHERE categoria = 'Eletrônicos';
```
4 - DELETE FROM: Exclui todos os produtos com preço inferior a 10.
```
DELETE FROM Produtos WHERE preco < 10;
```

## SQL Intermediário
| Comando           | Descrição                                                 |
|-------------------|-----------------------------------------------------------|
| HAVING            | Filtra grupos de resultados retornados por uma cláusula GROUP BY|
| AS                | Renomeia colunas ou tabelas em uma consulta               |
| BETWEEN           | Filtra resultados dentro de um intervalo específico      |
| LIKE              | Filtra resultados com base em um padrão de texto          |
| IN                | Determina se um valor especificado corresponde a qualquer valor em uma subconsulta ou lista especificada|
| EXISTS            | Testa se uma subconsulta retorna algum resultado          |
| UNION             | Combina o resultado de duas ou mais consultas em uma única consulta e remove duplicatas |
| UNION ALL         | Combina o resultado de duas ou mais consultas em uma única consulta, incluindo todas as linhas, mesmo que haja duplicatas |
| CASE              | Executa uma condição se uma ou mais condições forem verdadeiras |
| COALESCE          | Retorna o primeiro valor não nulo em uma lista de expressões|
| NULLIF            | Retorna NULL se os argumentos forem iguais, caso contrário, retorna o primeiro argumento|
| EXCEPT            | Retorna as linhas que são retornadas pela primeira consulta e não pela segunda consulta |
| INTERSECT         | Retorna as linhas que são retornadas por ambas as consultas |

### Alguns Exemplos práticos 
1 - HAVING: Seleciona a quantidade de pedidos para cada cliente, apenas para clientes com mais de 5 pedidos.
```
SELECT cliente_id, COUNT(*) as total_pedidos
FROM Pedidos
GROUP BY cliente_id
HAVING COUNT(*) > 5;
```
2 - AS: Seleciona o preço do produto com um rótulo mais descritivo.
```
SELECT nome AS produto, preco AS preco_unitario
FROM Produtos;
```
3 - BETWEEN: Seleciona todos os pedidos feitos entre duas datas.
```
SELECT * FROM Pedidos
WHERE data_pedido BETWEEN '2023-01-01' AND '2023-12-31';
```
4 - LIKE: Seleciona todos os produtos com o nome começando com 'Cam'.
```
SELECT * FROM Produtos WHERE nome LIKE 'Cam%';
```
5 - IN: Seleciona todos os produtos com categoria 'Eletrônicos' ou 'Roupas'.
```
SELECT * FROM Produtos WHERE categoria IN ('Eletrônicos', 'Roupas');
```
6 - EXISTS: Seleciona todos os clientes que fizeram pelo menos um pedido.
```
SELECT * FROM Clientes
WHERE EXISTS (SELECT 1 FROM Pedidos WHERE Pedidos.cliente_id = Clientes.cliente_id);
```
## SQL Avançado

| Comando           | Descrição                                                 |
|-------------------|-----------------------------------------------------------|
| WINDOW FUNCTIONS  | Realiza cálculos ou agregações em um conjunto de linhas relacionadas a uma linha de entrada específica |
| PARTITION BY      | Divide o conjunto de resultados em partições para as funções de janela|
| RANK              | Atribui uma classificação a cada linha de um conjunto de resultados com valores duplicados|
| DENSE_RANK        | Atribui uma classificação a cada linha de um conjunto de resultados, sem lacunas na classificação|
| LEAD              | Acessa o valor de uma linha próxima à linha atual em uma partição ordenada|
| LAG               | Acessa o valor de uma linha anterior à linha atual em uma partição ordenada|
| CROSS APPLY       | Aplica uma expressão de tabela de valor em cada linha de uma tabela de origem e retorna os resultados não vazios|
| PIVOT             | Rotaciona dados de uma coluna em múltiplas colunas em uma consulta de resultados|
| UNPIVOT           | Gira colunas de uma tabela em valores de linhas             |
| CTE (Common Table Expression) | Permite escrever uma consulta que se refere a si mesma, facilitando a escrita de consultas recursivas|
| MERGE             | Realiza operações INSERT, UPDATE ou DELETE em uma tabela de destino com base nos resultados de uma junção com outra tabela|
| TRIGGER           | Dispara uma ação quando um evento específico (INSERT, UPDATE, DELETE) ocorre em uma tabela|
| INDEX             | Melhora o desempenho de consultas, permitindo acesso rápido aos dados|
| TRANSACTION       | Agrupa uma série de operações de banco de dados em uma única unidade lógica de trabalho|

### Alguns Exemplos práticos
1 - WINDOW FUNCTIONS: Calcula a média de preço para cada categoria, junto com o preço máximo e mínimo.
```
SELECT categoria, AVG(preco) OVER (PARTITION BY categoria) AS media_preco,
       MAX(preco) OVER (PARTITION BY categoria) AS preco_maximo,
       MIN(preco) OVER (PARTITION BY categoria) AS preco_minimo
FROM Produtos;
```
2 - CTE (Common Table Expression): Seleciona todos os clientes que fizeram mais de 3 pedidos usando uma CTE.
```
WITH PedidosPorCliente AS (
    SELECT cliente_id, COUNT(*) AS total_pedidos
    FROM Pedidos
    GROUP BY cliente_id
)
SELECT * FROM PedidosPorCliente WHERE total_pedidos > 3;
```
3 - MERGE: Insere um novo cliente ou atualiza um existente se já estiver na tabela.
```
MERGE INTO Clientes AS target
USING (VALUES (123, 'João', 'joao@email.com')) AS source (cliente_id, nome, email)
ON target.cliente_id = source.cliente_id
WHEN MATCHED THEN
    UPDATE SET nome = source.nome, email = source.email
WHEN NOT MATCHED THEN
    INSERT (cliente_id, nome, email) VALUES (source.cliente_id, source.nome, source.email);
```
## Exemplo de como criar uma tabela com o nome 'usuarios'
````
CREATE TABLE usuarios (
    id INT PRIMARY KEY,
    nome VARCHAR(255),
    email VARCHAR(255),
    endereco VARCHAR(255),
    data_nascimento DATE
);
````
## Exemplo de como inserir dados na tabela 'usuarios'
```
INSERT INTO usuarios (nome, email, endereco, data_nascimento)VALUE
    ('João Silva', 'joao@example.com', 'Rua Principal, 123', '1990-05-15'),
    ('Maria Souza', 'maria@example.com', 'Avenida Central, 456', '1985-08-20'),
    ('Carlos Oliveira', 'carlos@example.com', 'Travessa das Flores, 789', '1992-12-10');
```
# OPERADORES LÓGICOS USADOS NO SQL
| Operador   | Descrição                                                     |
|------------|---------------------------------------------------------------|
| =          | Igual a                                                       |
| <> ou !=   | Diferente de                                                  |
| >          | Maior que                                                     |
| <          | Menor que                                                     |
| >=         | Maior ou igual a                                              |
| <=         | Menor ou igual a                                              |
| BETWEEN    | Entre dois valores específicos                                |
| LIKE       | Comparação de padrões de texto usando curingas                |
| IN         | Determina se um valor especificado corresponde a qualquer valor em uma subconsulta ou lista especificada|
| NOT        | Inverte o resultado de uma condição lógica                    |
| AND        | Retorna verdadeiro se todas as condições forem verdadeiras    |
| OR         | Retorna verdadeiro se pelo menos uma das condições for verdadeira|
| XOR        | Retorna verdadeiro se exatamente uma das condições for verdadeira|
| IS NULL    | Testa se um valor é nulo                                     |
| IS NOT NULL| Testa se um valor não é nulo                                 |

