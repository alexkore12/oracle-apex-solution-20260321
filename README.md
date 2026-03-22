# 🗄️ Oracle APEX Solution - PL/SQL Procedures

Colección de procedimientos, funciones y triggers PL/SQL para Oracle APEX.

## 📋 Descripción

Este proyecto contiene ejemplos de PL/SQL para Oracle APEX:
- **Procedures** - Procedimientos almacenados
- **Functions** - Funciones reutilizables
- **Triggers** - Automatización de eventos

## 🔐 Seguridad

### Mejores Prácticas PL/SQL

1. **Usar绑定变量 (Bind Variables)**
   - Siempre usar parámetros en lugar de concatenación
   - Previene SQL Injection

```sql
-- ✅ Correcto
CREATE PROCEDURE sp_get_customer (p_id IN NUMBER) AS
BEGIN
    SELECT * FROM customers WHERE id = p_id;
END;

-- ❌ Incorrecto - SQL Injection
CREATE PROCEDURE sp_get_customer (p_id IN VARCHAR2) AS
BEGIN
    EXECUTE IMMEDIATE 'SELECT * FROM customers WHERE id = ' || p_id;
END;
```

2. **Principio de Mínimo Privilegio**
   - Crear usuarios específicos para cada aplicación
   - No usar SYSTEM o SYS para aplicaciones

```sql
-- Crear usuario de aplicación con privilegios específicos
CREATE USER app_user IDENTIFIED BY "secure_password";
GRANT CONNECT, RESOURCE TO app_user;
GRANT EXECUTE ON sp_create_order TO app_user;
```

3. **Auditoría**
   - Habilitar auditoría para operaciones sensibles
   - Mantener logs de cambios

```sql
-- Auditoría de tabla
AUDIT INSERT, UPDATE, DELETE ON orders;
```

4. **Validar Entrada**
   - Siempre validar parámetros de entrada
   - Usar tipos de datos apropiados

```sql
CREATE PROCEDURE sp_create_order (
    p_id IN NUMBER,
    p_customer IN VARCHAR2,
    p_amount IN NUMBER
) AS
BEGIN
    -- Validar entrada
    IF p_amount < 0 THEN
        RAISE_APPLICATION_ERROR(-20001, 'Amount must be positive');
    END IF;
    
    IF p_customer IS NULL OR LENGTH(p_customer) > 100 THEN
        RAISE_APPLICATION_ERROR(-20002, 'Invalid customer name');
    END IF;
    
    -- Procedure logic
    INSERT INTO orders (id, customer, amount, created_date)
    VALUES (p_id, p_customer, p_amount, SYSDATE);
    COMMIT;
END;
```

5. **Evitar Texto Plano de Contraseñas**
   - Usar funciones hash para contraseñas
   - No almacenar contraseñas en texto plano

```sql
-- Usar DBMS_CRYPTO para hashing
CREATE OR REPLACE FUNCTION fn_hash_password (
    p_password IN VARCHAR2
) RETURN RAW AS
BEGIN
    RETURN DBMS_CRYPTO.HASH(
        UTL_RAW.CAST_TO_RAW(p_password),
        DBMS_CRYPTO.HASH_SHA256
    );
END;
```

## 🛠️ Componentes

### Procedures

| Procedure | Descripción |
|-----------|-------------|
| `sp_create_order` | Crea un nuevo pedido |

### Functions

| Function | Descripción |
|----------|-------------|
| `fn_get_total_orders` | Calcula total de pedidos por cliente |

### Triggers

| Trigger | Descripción |
|---------|-------------|
| `tr_order_audit` | Audita inserciones en orders |

## 🚀 Uso

### Ejecutar en SQL*Plus

```bash
sqlplus system/password@localhost:1521/orclpdb1
SQL> @plsql_procedures.sql
```

### Ejecutar en SQL Developer

1. Abre el archivo `plsql_procedures.sql`
2. Presiona F5 o Run Script

## 📝 Código

### Procedure: sp_create_order

```sql
CREATE OR REPLACE PROCEDURE sp_create_order (
    p_id IN NUMBER,
    p_customer IN VARCHAR2,
    p_amount IN NUMBER
) AS
BEGIN
    INSERT INTO orders (id, customer, amount, created_date)
    VALUES (p_id, p_customer, p_amount, SYSDATE);
    COMMIT;
END;
/
```

### Function: fn_get_total_orders

```sql
CREATE OR REPLACE FUNCTION fn_get_total_orders (
    p_customer IN VARCHAR2
) RETURN NUMBER AS
    v_total NUMBER;
BEGIN
    SELECT SUM(amount) INTO v_total
    FROM orders
    WHERE customer = p_customer;
    RETURN NVL(v_total, 0);
END;
/
```

### Trigger: tr_order_audit

```sql
CREATE OR REPLACE TRIGGER tr_order_audit
AFTER INSERT ON orders
FOR EACH ROW
BEGIN
    INSERT INTO order_audit (order_id, action, audit_date)
    VALUES (:NEW.id, 'INSERTED', SYSDATE);
END;
/
```

## 📋 Tablas Requeridas

```sql
-- Tabla de pedidos
CREATE TABLE orders (
    id NUMBER PRIMARY KEY,
    customer VARCHAR2(100),
    amount NUMBER(10,2),
    created_date DATE DEFAULT SYSDATE
);

-- Tabla de auditoría
CREATE TABLE order_audit (
    order_id NUMBER,
    action VARCHAR2(20),
    audit_date DATE
);
```

## 🔧 Ejemplos de Uso

### Llamar Procedure

```sql
BEGIN
    sp_create_order(
        p_id => 1,
        p_customer => 'Juan Pérez',
        p_amount => 150.00
    );
END;
/
```

### Usar Function

```sql
SELECT fn_get_total_orders('Juan Pérez') AS total
FROM dual;
```

### Ver Auditoría

```sql
SELECT * FROM order_audit ORDER BY audit_date DESC;
```

## 📁 Archivos

```
oracle-apex-solution-20260321/
├── plsql_procedures.sql  # Procedures, functions, triggers
├── SECURITY.md           # Guía de seguridad
└── README.md              # Este archivo
```

## 📝 Changelog

- **v1.1.0** - Agregadas mejores prácticas de seguridad PL/SQL
- **v1.0.0** - Versión inicial con ejemplos básicos

## 🤝 Contribución

¡Añade más procedures y mejora los existentes!

## 📄 Licencia

MIT License - Uso libre.
