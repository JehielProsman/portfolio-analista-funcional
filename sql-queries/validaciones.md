# Consultas SQL — Validación de Datos

**Contexto:** Consultas utilizadas durante el proceso de testing QA/UAT  
para validar integridad de datos entre módulos del sistema.  
**Autor:** Jehiel Prosman  
**Nota:** Datos y nombres de tablas anonimizados.

---

## QRY-001 — Verificar registros duplicados

Detecta registros duplicados en una tabla clave antes de una migración o release.

```sql
-- Seleccionamos el campo clave y contamos cuántas veces aparece
SELECT campo_clave, COUNT(*) AS cantidad

-- De la tabla principal del sistema
FROM tabla_principal

-- Agrupamos por campo_clave: así podemos contar cuántas filas tienen el mismo valor
GROUP BY campo_clave

-- HAVING filtra los grupos: solo nos muestra los que aparecen MÁS de una vez (= duplicados)
HAVING COUNT(*) > 1

-- Ordenamos de mayor a menor para ver primero los más duplicados
ORDER BY cantidad DESC;
```

**Cuándo se usa:** Antes de cada release para garantizar que no haya
datos duplicados que puedan romper la lógica del sistema.

---

## QRY-002 — Validar consistencia entre dos módulos

Detecta registros que existen en el módulo A pero no tienen
correspondencia en el módulo B — es decir, registros "huérfanos".

```sql
-- Traemos ID, descripción y fecha del módulo A
SELECT a.id, a.descripcion, a.fecha_creacion

-- Partimos desde el módulo A (tabla de origen)
FROM modulo_a a

-- LEFT JOIN: trae TODOS los registros de A, y los cruza con B si existe relación
-- Si no encuentra correspondencia en B, igual trae el registro de A (con NULLs en B)
LEFT JOIN modulo_b b ON a.id = b.id_referencia

-- Filtramos solo los que NO tienen correspondencia en B (id_referencia es NULL = no existe en B)
WHERE b.id_referencia IS NULL

-- Ordenamos por fecha para ver los más recientes primero
ORDER BY a.fecha_creacion DESC;
```

**Cuándo se usa:** Validar que toda transacción generada en un módulo
tenga su registro correspondiente en el módulo relacionado.

---

## QRY-003 — Verificar estados inválidos

Detecta registros que quedaron en un estado inconsistente,
por ejemplo cuando un proceso se interrumpió a mitad de camino.

```sql
-- Traemos los datos del registro: ID, estado actual, cuándo se modificó y quién lo hizo
SELECT id, estado, fecha_modificacion, usuario_modificacion

-- De la tabla que guarda los estados de los registros
FROM tabla_estados

-- Condición 1: el estado no es ninguno de los valores válidos del sistema
WHERE estado NOT IN ('ACTIVO', 'INACTIVO', 'PENDIENTE', 'CERRADO')

-- Condición 2 (OR): O bien está en PENDIENTE hace más de 24 horas (proceso colgado)
   OR (estado = 'PENDIENTE' AND fecha_modificacion < NOW() - INTERVAL 24 HOUR)

-- Ordenamos por fecha ascendente: los más viejos primero (llevan más tiempo en estado inválido)
ORDER BY fecha_modificacion ASC;
```

**Cuándo se usa:** Post-deploy para detectar registros que quedaron
en estados no contemplados por el sistema.

---

## QRY-004 — Auditoría de cambios recientes

Verifica qué registros fueron modificados en las últimas 24 horas.
Muy útil para validar el impacto real de un deploy.

```sql
-- Traemos todos los datos del log: qué cambió, de qué valor a qué valor, quién y cuándo
SELECT id, campo_modificado, valor_anterior, valor_nuevo,
       usuario, fecha_modificacion

-- De la tabla de auditoría, que registra automáticamente cada cambio en el sistema
FROM log_auditoria

-- Filtramos solo los cambios de las últimas 24 horas
-- NOW() es la fecha/hora actual; INTERVAL 24 HOUR resta 24 horas
WHERE fecha_modificacion >= NOW() - INTERVAL 24 HOUR

-- Ordenamos del más reciente al más viejo
ORDER BY fecha_modificacion DESC;
```

**Cuándo se usa:** Inmediatamente después de un deploy para confirmar
que solo se modificaron los datos esperados y nada más.

---

## QRY-005 — Contar registros por estado

Vista rápida del estado general de los datos — como un "semáforo" del sistema.

```sql
-- Traemos el estado, cuántos registros tiene cada uno, y qué porcentaje representa
SELECT estado, 
       COUNT(*) AS total,

       -- Calculamos el porcentaje: (cantidad de este estado / total general) * 100
       -- OVER() significa "sobre toda la tabla" — SUM con OVER() suma el total general
       -- ROUND(..., 2) redondea a 2 decimales
       ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER(), 2) AS porcentaje

-- De la tabla principal
FROM tabla_principal

-- Agrupamos por estado para contar cuántos hay de cada uno
GROUP BY estado

-- Ordenamos de mayor a menor cantidad
ORDER BY total DESC;
```

**Cuándo se usa:** Antes y después de un release para comparar
la distribución de estados y detectar si algo cambió inesperadamente.
ORDER BY total DESC;
```

**Cuándo se usa:** Antes y después de un release para comparar
la distribución de estados y detectar anomalías.
