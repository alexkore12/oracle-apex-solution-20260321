# Oracle APEX - PL/SQL Solutions

Colección completa de procedimientos, funciones y triggers para Oracle Database.

## 🚀 Características

- **Procedimientos** - sp_create_order y más
- **Funciones** - fn_get_total_orders
- **Triggers** - Auditoría automática
- **Oracle 19c+** - Compatible con Oracle Database
- **APEX** - Listo para Oracle APEX

## 📦 Contenido

### Procedimientos

| Procedure | Descripción |
|-----------|-------------|
| `sp_create_order` | Crear orden con validación |
| `sp_update_order` | Actualizar orden existente |
| `sp_delete_order` | Eliminar orden |

### Funciones

| Function | Descripción |
|----------|-------------|
| `fn_get_total_orders` | Calcular total de órdenes por cliente |
| `fn_get_order_count` | Contar órdenes |

### Triggers

| Trigger | Descripción |
|---------|-------------|
| `tr_order_audit` | Auditoría de cambios en órdenes |

## ⚙️ Instalación

### SQL*Plus

```bash
sqlplus user/password@//localhost:1521/orclpdb1
@plsql_procedures.sql
```

### Oracle APEX

1. Abrir SQL Workshop
2. Ir a Script Editor
3. Ejecutar `plsql_procedures.sql`

## 📖 Uso

### Crear Orden

```sql
EXEC sp_create_order(
    p_customer_id => 1,
    p_customer_name => 'Cliente Ejemplo',
    p_amount => 1000.00
);
```

### Calcular Total

```sql
SELECT fn_get_total_orders('Cliente Ejemplo') AS total
FROM DUAL;
-- Resultado: 1000.00
```

### Auditoría

Los triggers registran automáticamente:

- INSERT - Nueva orden creada
- UPDATE - Orden actualizada
- DELETE - Orden eliminada

```sql
-- Ver auditoría
SELECT * FROM order_audit_log
ORDER BY audit_timestamp DESC;
```

## 🔒 Mejores Prácticas PL/SQL

### Seguridad

1. **USING clause** - Siempre usar parámetros绑定
2. **Validación** - Verificar input antes de procesar
3. **Excepciones** - Manejo correcto de errores
4. **AUDIT** - Registrar operaciones sensibles

### Rendimiento

1. **Bulk Collect** - Para grandes volúmenes
2. **INDEX** - Índices en columnas frecuentes
3. **PL/SQL Result Cache** - Cachear funciones

## 🧪 Testing

```sql
-- Test procedure
BEGIN
    sp_create_order(1, 'Test', 500);
END;
/

-- Test function
SELECT fn_get_total_orders('Test') FROM DUAL;

-- Test trigger (verificar auditoría)
SELECT * FROM order_audit_log;
```

## 📝 Licencia

MIT - Alejandro Kore

## 🤖 Actualizado por

OpenClaw AI Assistant - 2026-03-21
*Mejoras: Documentación completa PL/SQL, mejores prácticas*
