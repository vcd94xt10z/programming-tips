# programming-tips
Dicas de programação em geral

## Estratégias para atualizar uma tabela de carga da forma mais rápida possível

Vamos descrever aqui algumas formas de atualizar uma tabela da forma mais rápido possível e com downtime mínimo
para quem estiver consultando as informações. Lembrando que essas tabelas são de carga de dados, ou seja, 
apenas 1 lugar atualiza e todos os outros só consultam informações.

O fluxo básico para a atualização é:
- Desabilitar auto commit
- Desabilitar chaves estrangeiras
- Desabilitar chaves unicas
- Iniciar a transação
- Aplicar as modificações no dados
- Atualizar os indices da tabela
- Habilitar tudo que foi desabilitado anteriormente

### MyISAM
- Suporta transação: Não
- Nível de Bloqueio: Tabela

### InnoDB
- Suporta transação: Sim
- Nível de Bloqueio: Linha 

## Outras informações
- Não executar comandos DDL dentro de uma transação
- LOAD DATA é mais rápido que insert
- Desabilitar verificações, indices etc antes de atualização pois se tiver habilitado, a cada modificação na linha
o banco terá que atualizar o indice. Os indices devem ser atualizados apenas após a atualização concluir

### Bloco padrão para desabilitar / habilitar as verificações

```sql
SET autocommit=0; 
SET unique_checks=0; 
SET foreign_key_checks=0;

ALTER TABLE `table1` DISABLE KEYS;

BEGIN;
... atualização aqui ...
COMMIT;

ALTER TABLE `table1` ENABLE KEYS;

OPTIMIZE TABLE table1;

SET autocommit=0;
SET unique_checks=0;
SET foreign_key_checks=0;
```

### Trocando os dados de uma tabela

```sql
SET autocommit=0; 
SET unique_checks=0; 
SET foreign_key_checks=0;

-- tabela temporária
CREATE TEMPORATY TABLE table2 LIKE table1;

-- inserindo na tabela temporária
BEGIN;
INSERT INTO table2 VALUES (...);
INSERT INTO table2 VALUES (...);
INSERT INTO table2 VALUES (...);
COMMIT;

ALTER TABLE `table1` DISABLE KEYS;

-- limpando e atualizando a tabela principal
BEGIN;
DELETE FROM table1;
INSERT INTO table1 SELECT * FROM table2;
COMMIT;

ALTER TABLE `table1` ENABLE KEYS;

OPTIMIZE TABLE table1;

SET autocommit=1;
SET unique_checks=1;
SET foreign_key_checks=1;
```

### Atualizar uma tabela inteira (mantendo os dados existentes) 
```sql
SET autocommit=0; 
SET unique_checks=0; 
SET foreign_key_checks=0;

CREATE TEMPORATY TABLE table2 LIKE table1;

BEGIN;
INSERT INTO table2 SELECT * FROM table1;
REPLACE INTO table2 VALUES (...);
REPLACE INTO table2 VALUES (...);
REPLACE INTO table2 VALUES (...);
COMMIT;

ALTER TABLE `table1` DISABLE KEYS;

BEGIN;
DELETE FROM table1;
INSERT INTO table1 SELECT * FROM table2;
COMMIT;

ALTER TABLE `table1` ENABLE KEYS;

OPTIMIZE TABLE table1;

SET autocommit=1;
SET unique_checks=1;
SET foreign_key_checks=1; 
```

### Trabalhando com versões (modo 1)

```sql
-- Os campos versao e chave são PKs

SET autocommit=0; 
SET unique_checks=0; 
SET foreign_key_checks=0;

ALTER TABLE `table1` DISABLE KEYS;

BEGIN
INSERT INTO table1 (chave,versao,...) VALUES ('1',2,...);
INSERT INTO table1 (chave,versao,...) VALUES ('2',2,...);
INSERT INTO table1 (chave,versao,...) VALUES ('3',2,...);
DELETE FROM table1 WHERE versao <> 2;
COMMIT;

ALTER TABLE `table1` ENABLE KEYS;

OPTIMIZE TABLE table1;

SET autocommit=1;
SET unique_checks=1;
SET foreign_key_checks=1; 
```

### Trabalhando com versões (modo 2)

```sql
-- Os campos versao e chave são PKs

SET autocommit=0; 
SET unique_checks=0; 
SET foreign_key_checks=0;

ALTER TABLE `table1` DISABLE KEYS;

BEGIN
INSERT INTO table1 (chave,versao,...) VALUES ('1',2,...);
INSERT INTO table1 (chave,versao,...) VALUES ('2',2,...);
INSERT INTO table1 (chave,versao,...) VALUES ('3',2,...);
COMMIT;

BEGIN;
DELETE t1 FROM table1 t1, table1 t2 
WHERE t1.versao > t2.versao AND t1.chave = t2.chave
COMMIT;

ALTER TABLE `table1` ENABLE KEYS;

OPTIMIZE TABLE table1;

SET autocommit=1;
SET unique_checks=1;
SET foreign_key_checks=1; 
```