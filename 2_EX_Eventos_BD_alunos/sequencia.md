# Resolução

## Implementação SQL

### 1. Criação do Banco de Dados

```sql
CREATE DATABASE IF NOT EXISTS eventos_db;
USE eventos_db;

```

### 2. Criação das Tabelas

```sql
-- Tabela Palestrantes
CREATE TABLE Palestrantes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    especialidade VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE
);

-- Tabela Eventos
CREATE TABLE Eventos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    titulo VARCHAR(255) NOT NULL,
    data_evento DATE NOT NULL,
    local VARCHAR(255) NOT NULL,
    capacidade INT NOT NULL DEFAULT 50,
    palestrante_id INT NOT NULL,
    FOREIGN KEY (palestrante_id) REFERENCES Palestrantes(id)
        ON DELETE RESTRICT
        ON UPDATE CASCADE
);

-- Tabela Inscricoes
CREATE TABLE Inscricoes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    evento_id INT NOT NULL,
    nome_participante VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    data_inscricao DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    presente BOOLEAN NOT NULL DEFAULT FALSE,
    FOREIGN KEY (evento_id) REFERENCES Eventos(id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);

-- Índice para evitar inscrições duplicadas no mesmo evento pelo mesmo email (opcional)
CREATE UNIQUE INDEX idx_evento_email ON Inscricoes(evento_id, email);





```

### 3. Inserção de Dados Iniciais

```sql
-- Inserir palestrantes



-- Inserir eventos



-- Inserir algumas inscrições



```

### 4. View para Consulta

```sql
-- View para listar eventos com detalhes
CREATE VIEW vw_eventos_detalhados AS
SELECT 
    e.id,
    e.titulo,
    e.data_evento,
    e.local,
    e.capacidade,
    p.nome AS palestrante,
    p.especialidade,
    COUNT(i.id) AS total_inscritos,
    e.capacidade - COUNT(i.id) AS vagas_disponiveis
FROM 
    eventos e
LEFT JOIN 
    palestrantes p ON e.palestrante_id = p.id
LEFT JOIN 
    inscricoes i ON e.id = i.evento_id
GROUP BY 
    e.id, e.titulo, e.data_evento, e.local, e.capacidade, p.nome, p.especialidade;

-- Consultar a view
SELECT * FROM vw_eventos_detalhados;
```

### 5. Consultas Úteis

```sql
-- 1. Listar todos os eventos com suas respectivas informações



-- 2. Mostrar apenas eventos com vagas disponíveis (usando a view criada)
SELECT id, titulo, data_evento, local, vagas_disponiveis 
FROM vw_eventos_detalhados
WHERE vagas_disponiveis > 0;

-- 3. Listar participantes inscritos em um evento específico



```
