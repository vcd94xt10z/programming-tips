# programming-tips
Dicas de programação

## Estratégias para atualizar uma tabela da forma mais rápida possível

Vamos descrever aqui algumas formas de atualizar uma tabela da forma mais rápido possível e sem downtime para quem
estiver consultando as informações.

### Trocando os dados de uma tabela

```sql
BEGIN
CREATE TEMPORATY TABLE table2 LIKE table1;
INSERT INTO table2 VALUES (...);
INSERT INTO table2 VALUES (...);
INSERT INTO table2 VALUES (...);
DELETE FROM table1;
INSERT INTO table1 SELECT * FROM table2;
COMMIT;
OPTIMIZE TABLE table1; 
```

### Atualizar uma tabela inteira (mantendo os dados existentes) 
```sql
BEGIN
CREATE TEMPORATY TABLE table2 LIKE table1;
INSERT INTO table2 SELECT * FROM table1;
REPLACE INTO table2 VALUES (...);
REPLACE INTO table2 VALUES (...);
REPLACE INTO table2 VALUES (...);
DELETE FROM table1;
INSERT INTO table1 SELECT * FROM table2;
COMMIT;
OPTIMIZE TABLE table1; 
```

### Trabalhando com versões

```sql
BEGIN
INSERT INTO table1 (versao,chave,...) VALUES (2,'1',...);
INSERT INTO table1 (versao,chave,...) VALUES (2,'2',...);
INSERT INTO table1 (versao,chave,...) VALUES (2,'3',...);
COMMIT;
OPTIMIZE TABLE table1;


-- para consultar
SELECT *
FROM table1
WHERE versao = (SELECT MAX(versao) FROM table1)
```