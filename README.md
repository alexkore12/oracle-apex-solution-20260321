# Oracle APEX - PL/SQL Solutions

## Descripción
Procedimientos, funciones y triggers para Oracle Database.

## Contenido
- sp_create_order: Procedure para crear órdenes
- fn_get_total_orders: Function para calcular total
- tr_order_audit: Trigger de auditoría

## Uso
```sql
-- Ejecutar en SQL Developer o SQL*Plus
@plsql_procedures.sql
```

## Testing
```sql
-- Probar procedure
EXEC sp_create_order(1, 'Cliente', 1000);

-- Probar function
SELECT fn_get_total_orders('Cliente') FROM DUAL;
```
