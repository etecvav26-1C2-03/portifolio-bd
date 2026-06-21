# SQL — Comandos Básicos

## DDL (estrutura do banco)

```sql
-- Criar tabela
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    data_nascimento DATE
);

-- Deletar tabela
DROP TABLE clientes;

-- Apagar dados mas manter estrutura
TRUNCATE TABLE clientes;
```

---

## DML (manipular dados)

```sql
-- Inserir registro
INSERT INTO clientes (id, nome, email) VALUES (1, 'Ana', 'ana@email.com');

-- Buscar dados
SELECT * FROM clientes;
SELECT nome, email FROM clientes WHERE id = 1;

-- Atualizar registro
UPDATE clientes SET email = 'novo@email.com' WHERE id = 1;

-- Deletar registro
DELETE FROM clientes WHERE id = 1;
```

---

## ALTER TABLE

```sql
-- Adicionar coluna
ALTER TABLE clientes ADD COLUMN telefone VARCHAR(15);

-- Remover coluna
ALTER TABLE clientes DROP COLUMN telefone;

-- Renomear coluna
ALTER TABLE clientes RENAME COLUMN nome TO nome_completo;

-- Mudar tipo de dado de uma coluna
ALTER TABLE clientes MODIFY COLUMN email TEXT;         -- MySQL
ALTER TABLE clientes ALTER COLUMN email TYPE TEXT;     -- PostgreSQL

-- Adicionar constraint NOT NULL
ALTER TABLE clientes MODIFY COLUMN nome VARCHAR(100) NOT NULL;   -- MySQL
ALTER TABLE clientes ALTER COLUMN nome SET NOT NULL;             -- PostgreSQL

-- Remover NOT NULL
ALTER TABLE clientes ALTER COLUMN nome DROP NOT NULL;  -- PostgreSQL

-- Adicionar PRIMARY KEY
ALTER TABLE clientes ADD PRIMARY KEY (id);

-- Adicionar FOREIGN KEY
ALTER TABLE pedidos ADD CONSTRAINT fk_cliente
    FOREIGN KEY (cliente_id) REFERENCES clientes(id);

-- Remover FOREIGN KEY
ALTER TABLE pedidos DROP FOREIGN KEY fk_cliente;      -- MySQL
ALTER TABLE pedidos DROP CONSTRAINT fk_cliente;       -- PostgreSQL

-- Renomear tabela
ALTER TABLE clientes RENAME TO usuarios;              -- PostgreSQL / SQLite
RENAME TABLE clientes TO usuarios;                    -- MySQL
```

---

## Cláusulas comuns no SELECT

```sql
SELECT nome FROM clientes
WHERE cidade = 'Jundiaí'
ORDER BY nome ASC
LIMIT 10;

-- Agrupar e contar
SELECT cidade, COUNT(*) AS total
FROM clientes
GROUP BY cidade
HAVING COUNT(*) > 5;
```

---

## Joins básicos

```sql
-- INNER JOIN — só registros que casam nas duas tabelas
SELECT c.nome, p.valor
FROM clientes c
INNER JOIN pedidos p ON c.id = p.cliente_id;

-- LEFT JOIN — todos da esquerda, mesmo sem par na direita
SELECT c.nome, p.valor
FROM clientes c
LEFT JOIN pedidos p ON c.id = p.cliente_id;
```

---

## Observações

- `DROP` vs `DELETE`: `DROP TABLE` apaga a tabela inteira (estrutura + dados). `DELETE` apaga linhas, mas a tabela continua existindo.
- `TRUNCATE` vs `DELETE`: `TRUNCATE` é mais rápido pra limpar tudo, mas geralmente não pode ser revertido com rollback.
- Sintaxe de `ALTER TABLE` varia entre MySQL, PostgreSQL e SQLite — os comentários acima indicam onde há diferença.
