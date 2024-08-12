Indexação em Oracle

Tipos de Índices em Oracle:

             B-tree Index:

                É o tipo de índice mais comum no Oracle.
                Organiza os dados de forma hierárquica, permitindo uma busca rápida.
                É eficiente em consultas que utilizam operadores de igualdade ou intervalo.
                Adequado para colunas que possuem uma ampla variedade de valores distintos.
    
            Bitmap Index:

                Usa mapas de bits para indexar valores de colunas.
                É eficiente em colunas com um pequeno número de valores distintos (baixa cardinalidade).
                Frequentemente utilizado em sistemas de data warehousing.
                Pode ser combinado com outras colunas para melhorar a performance de consultas complexas.
            
            Function-based Index:

                Permite a criação de índices em expressões ou funções baseadas em colunas.
                Útil quando consultas utilizam funções específicas sobre colunas, como UPPER(column_name).
                Pode melhorar a performance de consultas que utilizam essas funções

Impacto dos Índices na Performance de Consultas:

                Melhora no Tempo de Resposta:
                Índices podem acelerar significativamente o tempo de resposta das consultas, especialmente em grandes volumes de dados.

                Custo de Manutenção:
                Índices consomem espaço adicional em disco e podem impactar o desempenho de operações de escrita, como INSERT, UPDATE e DELETE.

                Escolha do Índice:
                Escolher o tipo correto de índice para uma coluna pode otimizar a performance; no entanto, o uso indevido pode levar a um desempenho degradado.

Projeto Prático
Passo 1: Criação de uma Tabela de Teste
```sql
CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50),
    last_name VARCHAR2(50),
    department_id NUMBER,
    salary NUMBER
);
```

Passo 2: População da Tabela com Dados

```sql
INSERT INTO employees (employee_id, first_name, last_name, department_id, salary)
VALUES (1, 'John', 'John', 10, 5000);
-- Repita com diferentes valores para adicionar mais registros
```

Passo 3: Criação de Índices

Índice B-tree:
```sql
CREATE INDEX idx_employee_last_name ON employees(last_name);
```
Índice Bitmap:
```sql
CREATE BITMAP INDEX idx_employee_department ON employees(department_id);
```
Índice Function-based:
```sql
CREATE INDEX idx_employee_upper_last_name ON employees(UPPER(last_name));
```
Teste de Consultas com e sem Índices

Consulta sem índice
```sql
EXPLAIN PLAN FOR 
SELECT * FROM employees WHERE last_name = 'Doe';
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```
Consulta com índice B-tree:
```sql
EXPLAIN PLAN FOR 
SELECT * FROM employees WHERE last_name = 'Doe';
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```
Consulta com índice Function-based:
```sql
EXPLAIN PLAN FOR 
SELECT * FROM employees WHERE UPPER(last_name) = 'DOE';
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```
Comparação de Planos de Execução

Observe como o otimizador usa os diferentes tipos de índices e compare o custo das consultas para entender o impacto de cada tipo de índice na performance.

Conclusão
A indexação é uma técnica poderosa para otimizar a performance de consultas em bancos de dados Oracle. A escolha correta do tipo de índice — seja B-tree, Bitmap ou Function-based — pode fazer uma diferença significativa no tempo de resposta das consultas, especialmente em grandes volumes de dados.

Durante o projeto, observou-se que:

        Índices B-tree são eficazes em colunas com alta cardinalidade, onde os valores são amplamente distintos. Eles são versáteis e funcionam bem com operadores de igualdade e intervalos.

        Índices Bitmap são mais eficientes em colunas com baixa cardinalidade, onde há poucos valores distintos. Eles são particularmente úteis em ambientes de data warehousing, onde consultas complexas e agregações são comuns.

        Índices Function-based mostraram-se valiosos em consultas que aplicam funções ou expressões em colunas. Ao criar um índice baseado na função, a performance dessas consultas foi significativamente melhorada.

Por fim, o uso de índices deve ser cuidadosamente planejado. Embora possam acelerar a performance de leitura, há um custo associado à manutenção de índices, especialmente em operações de escrita. Além disso, o uso excessivo ou inadequado de índices pode levar a um desempenho degradado, destacando a importância de uma análise criteriosa antes de sua implementação.

Este projeto fornece uma visão prática de como os índices funcionam em Oracle, ilustrando a importância de entender as características dos dados e das consultas ao projetar estratégias de indexação.
