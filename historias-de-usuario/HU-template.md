# Template — Historia de Usuario

**Proyecto:** [Nombre del proyecto]  
**Módulo:** [Nombre del módulo]  
**Fecha:** [DD/MM/AAAA]  
**Autor:** Jehiel Prosman  

---

## Identificación

| Campo | Detalle |
|---|---|
| **ID** | HU-XXX |
| **Épica** | [Nombre de la épica] |
| **Prioridad** | Alta / Media / Baja |
| **Estado** | Draft / En revisión / Aprobada |

---

## Historia

**Como** [tipo de usuario]  
**Quiero** [acción que desea realizar]  
**Para** [beneficio o valor que obtiene]

---

## Criterios de Aceptación

| # | Criterio | Condición |
|---|---|---|
| 1 | Dado que [contexto], cuando [acción], entonces [resultado esperado] | ✅ |
| 2 | Dado que [contexto], cuando [acción], entonces [resultado esperado] | ✅ |
| 3 | Dado que [contexto], cuando [acción], entonces [resultado esperado] | ✅ |

---

## Reglas de Negocio

- RN-01: [Descripción de la regla]
- RN-02: [Descripción de la regla]

---

## Notas / Aclaraciones

- [Cualquier aclaración relevante para el equipo de desarrollo]

---

## Ejemplo aplicado

**Como** médico del sistema  
**Quiero** poder ver el historial de turnos de un paciente  
**Para** tener contexto antes de una consulta

### Criterios:
1. Dado que el médico está autenticado, cuando busca un paciente por nombre o DNI, entonces el sistema muestra su historial de turnos ordenado por fecha descendente.
2. Dado que el paciente no tiene turnos previos, cuando el médico lo busca, entonces el sistema muestra el mensaje "Sin turnos registrados".
3. Dado que el médico no tiene permisos sobre ese paciente, cuando intenta acceder, entonces el sistema deniega el acceso y registra el intento.
