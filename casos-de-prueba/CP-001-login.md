# Casos de Prueba — Módulo: Autenticación de Usuarios

**Sistema:** Sistema de gestión (datos anonimizados)  
**Módulo:** Login / Autenticación  
**Tester:** Jehiel Prosman  
**Herramienta:** Jira / Zephyr  

---

## CP-001 — Login con credenciales válidas

| Campo | Detalle |
|---|---|
| **ID** | CP-001 |
| **Módulo** | Autenticación |
| **Tipo** | Funcional positivo |
| **Prioridad** | Alta |

**Precondiciones:**
- El usuario existe en el sistema con rol activo
- El sistema está disponible

**Pasos:**
1. Ingresar al sistema
2. Completar campo Usuario con credencial válida
3. Completar campo Contraseña correcta
4. Click en "Ingresar"

**Resultado esperado:**
- El sistema autentica al usuario
- Redirige al panel principal según su rol
- Se registra el acceso en el log del sistema

**Resultado obtenido:** ✅ Conforme

---

## CP-002 — Login con contraseña incorrecta

| Campo | Detalle |
|---|---|
| **ID** | CP-002 |
| **Módulo** | Autenticación |
| **Tipo** | Funcional negativo |
| **Prioridad** | Alta |

**Precondiciones:**
- El usuario existe en el sistema

**Pasos:**
1. Ingresar al sistema
2. Completar campo Usuario con credencial válida
3. Completar campo Contraseña **incorrecta**
4. Click en "Ingresar"

**Resultado esperado:**
- El sistema rechaza el acceso
- Muestra mensaje de error claro al usuario
- No redirige al panel principal

**Resultado obtenido:** ✅ Conforme

---

## CP-003 — Login con usuario inexistente

| Campo | Detalle |
|---|---|
| **ID** | CP-003 |
| **Módulo** | Autenticación |
| **Tipo** | Funcional negativo |
| **Prioridad** | Media |

**Pasos:**
1. Ingresar al sistema
2. Completar campo Usuario con un valor que no existe en el sistema
3. Click en "Ingresar"

**Resultado esperado:**
- El sistema rechaza el acceso
- Muestra mensaje genérico (no debe indicar si el usuario existe o no)

**Resultado obtenido:** ✅ Conforme

