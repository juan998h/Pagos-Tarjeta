# 🏦 BancoPay — Sistema de Gestión de Pagos con Tarjeta

> **Proyecto de Ingeniería de Software — 2025**  
> Aplicación web SPA · HTML5 + CSS3 + JavaScript Vanilla · Sin dependencias externas

---

## 📋 Tabla de Contenidos

1. [Descripción General](#descripción-general)
2. [Instrucciones para Ejecutar](#instrucciones-para-ejecutar)
3. [Usuarios de Prueba (Demo)](#usuarios-de-prueba-demo)
4. [Escenarios de Prueba](#escenarios-de-prueba)
5. [Funcionalidades Implementadas](#funcionalidades-implementadas)
6. [Modelos del Dominio](#modelos-del-dominio)
7. [Trazabilidad Modelo → Código](#trazabilidad-modelo--código)
8. [Arquitectura del Sistema](#arquitectura-del-sistema)
9. [Convenciones del Repositorio](#convenciones-del-repositorio)

---

## Descripción General

BancoPay es una aplicación web de página única (SPA) que simula el sistema de pagos con tarjeta de crédito de una entidad bancaria. Implementa las dos pantallas funcionales requeridas para el MVP:

| Pantalla | Función | Entidad del Dominio |
|----------|---------|---------------------|
| **Pantalla 1** | Formulario de pago con tarjeta — transacción principal del negocio | Pago / Transacción |
| **Pantalla 2** | CRUD de tarjetas de crédito — listar, crear, editar y eliminar | TarjetaCredito |

### Características clave

- Validación en tiempo real con 9 reglas de negocio visibles en panel de seguridad
- Verificación de tarjeta contra la base de datos interna del sistema
- Descuento automático del saldo disponible al aprobar un pago
- CVV con botón de ojo para mostrar/ocultar en formulario y en el CRUD
- Historial de transacciones por tarjeta (aprobadas y rechazadas)
- Sin dependencias externas — un solo archivo HTML, sin instalación requerida

---

## Instrucciones para Ejecutar

### Requisitos

| Herramienta | Requisito |
|-------------|-----------|
| Navegador web | Chrome, Firefox, Edge o Safari (versión moderna) |
| Servidor / instalación | ❌ No se requiere |
| Internet | ❌ No se requiere (después de la primera carga de fuentes) |

### Pasos

1. Clone o descargue el repositorio en su computador
2. Ubique el archivo `banco_mvp_v2.html` en la carpeta del proyecto
3. Haga **doble clic** sobre el archivo para abrirlo en el navegador
4. La aplicación carga de inmediato con los 15 registros de prueba precargados
5. Use la barra de navegación superior para cambiar entre las dos pantallas

### Abrir desde terminal

```bash
# Windows
start banco_mvp_v2.html

# macOS
open banco_mvp_v2.html

# Linux
xdg-open banco_mvp_v2.html
```

### Estructura del proyecto

```
bancopay/
├── banco_mvp_v2.html   ← Aplicación completa (HTML + CSS + JS en un solo archivo)
└── README.md           ← Este archivo
```

> ⚠️ **Nota sobre persistencia:** Los datos se almacenan en memoria RAM del navegador. Al recargar la página los datos vuelven al estado inicial. Comportamiento esperado para un MVP sin backend.

---

## Usuarios de Prueba (Demo)

El sistema no requiere autenticación. Los siguientes datos de tarjeta están precargados al abrir la aplicación:

| # | Titular | Número | Tipo | CVV | Vence | Saldo Disponible | Estado |
|---|---------|--------|------|-----|-------|-----------------|--------|
| 1 | ANA MILENA RIOS | 4532 1234 5678 9010 | VISA | 321 | 03/26 | $4.800.000 | ✅ Activa |
| 2 | CARLOS GUTIERREZ | 5412 7534 1234 5678 | MasterCard | 456 | 07/25 | $6.500.000 | ✅ Activa |
| 3 | LAURA OSPINA | 3714 496353 98431 | AMEX | 2048 | 11/24 | $0 | ⚫ Vencida |
| 4 | MIGUEL TORRES | 4111 1111 1111 1111 | VISA | 789 | 09/27 | $3.500.000 | 🔴 Bloqueada |
| 5 | SOFIA VARGAS | 5500 0055 5555 5559 | MasterCard | 654 | 01/26 | $5.200.000 | ✅ Activa |
| 6 | ANDRES MEJIA | 4716 2394 7652 8012 | VISA | 147 | 06/26 | $3.900.000 | ✅ Activa |
| 7 | VALENTINA CRUZ | 5105 1051 0510 5100 | MasterCard | 258 | 12/23 | $0 | ⚫ Vencida |
| 8 | JORGE CARDENAS | 4539 5786 0512 3651 | VISA | 963 | 04/28 | $14.200.000 | ✅ Activa |
| 9 | MARIA JOSE LEON | 4000 0000 0000 0002 | VISA | 852 | 08/26 | $2.500.000 | 🔴 Bloqueada |
| 10 | PABLO MORENO | 5454 5454 5454 5454 | MasterCard | 741 | 10/25 | $7.800.000 | ✅ Activa |
| 11 | CAROLINA PENA | 4916 3384 0547 9413 | VISA | 159 | 02/27 | $5.000.000 | ✅ Activa |
| 12 | SANTIAGO HERRERA | 5425 2334 3010 9903 | MasterCard | 357 | 05/26 | $9.500.000 | ✅ Activa |
| 13 | DANIELA MOLINA | 4917 6100 0000 0000 | VISA | 486 | 03/25 | $3.000.000 | 🔴 Bloqueada |
| 14 | JUAN PABLO CASTRO | 4462 0300 0000 0000 | VISA | 753 | 07/28 | $18.000.000 | ✅ Activa |
| 15 | ISABELLA JIMENEZ | 5425 2334 3010 9999 | MasterCard | 624 | 09/26 | $7.200.000 | ✅ Activa |

---

## Escenarios de Prueba

### ✅ Escenario 1 — Pago exitoso

```
Número:      4532 1234 5678 9010
Nombre:      ANA MILENA RIOS
Vencimiento: 03/26
CVV:         321
Monto:       500000
Concepto:    Compra en línea
```
**Resultado esperado:** APROBADO — saldo pasa de $4.800.000 a $4.300.000. Se genera código de autorización.

---

### ❌ Escenario 2 — CVV incorrecto

```
Número:  5412 7534 1234 5678
CVV:     000   ← incorrecto (el real es 456)
Monto:   200000
```
**Resultado esperado:** RECHAZADO — "CVV incorrecto". Queda registrado en el historial de la tarjeta.

---

### ❌ Escenario 3 — Tarjeta bloqueada

```
Número: 4111 1111 1111 1111  (Miguel Torres)
```
**Resultado esperado:** Badge naranja "Tarjeta BLOQUEADA". El botón Procesar permanece deshabilitado.

---

### ❌ Escenario 4 — Tarjeta vencida

```
Número: 3714 496353 98431  (Laura Ospina)
```
**Resultado esperado:** Badge gris "Tarjeta VENCIDA". No puede realizar transacciones.

---

### ❌ Escenario 5 — Saldo insuficiente

```
Número: 4539 5786 0512 3651  (Jorge Cardenas — saldo $14.200.000)
CVV:    963
Monto:  20000000
```
**Resultado esperado:** Validación de monto falla en rojo. El botón permanece deshabilitado.

---

### ❌ Escenario 6 — Tarjeta no registrada en el sistema

```
Número: 4111 2222 3333 4444  (no existe)
```
**Resultado esperado:** Badge rojo "Tarjeta no registrada en el sistema". Validación falla.

---

## Funcionalidades Implementadas

### Pantalla 1 — Formulario de Pago

El formulario implementa **9 validaciones en tiempo real** visibles en el panel de seguridad:

| # | Validación | Descripción | Resultado si falla |
|---|-----------|-------------|-------------------|
| 1 | Formato 16 dígitos | El número debe tener exactamente 16 dígitos numéricos | Campo en rojo |
| 2 | Algoritmo Luhn | Verifica que el número sea matemáticamente legítimo | Indicador FAIL |
| 3 | Tarjeta registrada | Busca el número en la base de datos interna | Badge rojo |
| 4 | Estado activa | Verifica que no esté bloqueada ni vencida | Badge naranja/gris |
| 5 | Fecha de vencimiento | Formato MM/YY y cruzado con fecha real de la BD | Error de fecha |
| 6 | CVV válido (formato) | 3 o 4 dígitos numéricos | Error campo CVV |
| 7 | Monto válido | Mínimo $1.000, máximo $50.000.000 y ≤ saldo disponible | Error monto |
| 8 | Nombre titular | Mínimo 3 caracteres | Error nombre |
| 9 | Nombre coincide BD | Al menos una palabra debe coincidir con el titular registrado | Validación FAIL |

**Flujo de aprobación:** Si las 9 validaciones pasan Y el CVV coincide con el registrado → se descuenta el saldo, se registra la transacción como APROBADA, se genera código de autorización y se muestra el recibo.

**Flujo de rechazo:** Si el CVV no coincide → se registra la transacción como RECHAZADA con el motivo. El saldo NO se modifica.

---

### Pantalla 2 — CRUD de Tarjetas

| Operación | Descripción | Validaciones |
|-----------|-------------|-------------|
| **Listar (R)** | Tabla completa con colores por estado. Columnas: titular, número, tipo, vencimiento, CVV (oculto/visible), saldo, límite, estado, acciones. Búsqueda en tiempo real y filtros por estado. | — |
| **Crear (C)** | Formulario inline con todos los campos. | Número único, Luhn, CVV 3-4 dígitos, campos obligatorios |
| **Actualizar (U)** | Formulario pre-poblado al hacer clic en Editar. Permite cambiar nombre, vencimiento, CVV, límite y estado. | CVV válido. Ajuste automático de saldo si aumenta el límite |
| **Eliminar (D)** | Botón con confirmación. Elimina también el historial de transacciones. | Confirmación obligatoria del usuario |

**Funcionalidad extra — CVV visible/oculto:** Cada fila muestra el CVV enmascarado (•••). Un botón 👁 permite revelarlo por tarjeta individual.

**Funcionalidad extra — Historial por cliente:** El botón 📋 en cada fila abre un panel con el historial completo de esa tarjeta mostrando: resumen estadístico, lista de transacciones con concepto, fecha, monto, estado y motivo de rechazo.

---

## Modelos del Dominio

### Modelo 1: TarjetaCredito

```javascript
{
  id:      Number,  // Identificador único autoincremental
  name:    String,  // Nombre del titular en mayúsculas
  num:     String,  // Número formateado con espacios (e.g. "4532 1234 5678 9010")
  type:    String,  // Franquicia: "VISA" | "MasterCard" | "AMEX"
  exp:     String,  // Vencimiento en formato "MM/YY"
  cvv:     String,  // Código de seguridad 3-4 dígitos (campo sensible)
  limit:   Number,  // Límite máximo de crédito en COP
  balance: Number,  // Saldo disponible actual en COP (se actualiza con cada pago)
  status:  String,  // Estado: "activa" | "bloqueada" | "vencida"
}
```

### Modelo 2: Transaccion (Pago)

```javascript
{
  id:      String,  // Código único "TXN001", "TXN002", ...
  concept: String,  // Concepto o descripción del pago
  amount:  Number,  // Monto de la transacción en COP
  status:  String,  // "aprobado" | "rechazado"
  date:    String,  // Fecha y hora formateada
  cuotas:  String,  // Número de cuotas (solo transacciones aprobadas)
  reason:  String,  // Motivo del rechazo (solo transacciones rechazadas)
}
```

### Modelo 3: Repositorio de Transacciones

```javascript
// Objeto indexado por ID de tarjeta
transactions = {
  1:  [ { id, concept, amount, status, date }, ... ],  // Ana Milena Rios
  2:  [ { id, concept, amount, status, date }, ... ],  // Carlos Gutierrez
  // ...
}
```

---

## Trazabilidad Modelo → Código

| Elemento del Dominio | Función / Variable JS | Capa | Descripción |
|---------------------|----------------------|------|-------------|
| `TarjetaCredito[]` | `cards` | Data Layer | Array global con los 15 objetos tarjeta precargados |
| `Transaccion{}` | `transactions` | Data Layer | Objeto indexado por `cardId` con arrays de transacciones |
| Buscar tarjeta | `findCardByNum(num)` | Service Layer | Lookup por número, retorna el objeto tarjeta o null |
| Validar pago | `validatePago()` | Service Layer | 9 validaciones en cascada con feedback visual en tiempo real |
| Procesar pago | `procesarPago()` | Service Layer | Descuenta saldo, registra transacción, muestra recibo |
| Listar tarjetas | `renderTable()` | UI Layer | Genera HTML dinámico en el `tbody` de la tabla |
| Estadísticas | `renderStats()` | UI Layer | Calcula y muestra métricas agregadas |
| Crear tarjeta | `createCard()` | CRUD Layer | Valida y agrega nuevo objeto al array `cards` |
| Editar tarjeta | `saveEdit()` | CRUD Layer | Actualiza el objeto buscado por `id` en `cards[]` |
| Eliminar tarjeta | `deleteCard(id)` | CRUD Layer | Filtra el array y elimina `transactions[id]` |
| Ver transacciones | `showTxnPanel(id)` | UI Layer | Renderiza historial de la tarjeta seleccionada |
| Toggle CVV | `toggleRowCvv(id, cvv)` | UI Layer | Muestra u oculta el CVV en la fila de la tabla |
| Algoritmo Luhn | `luhn(num)` | Service Layer | Valida matemáticamente el número de tarjeta |

### Flujo completo de una transacción

```
1. Usuario ingresa número de tarjeta
        ↓
2. formatCardNum() → formatea y detecta franquicia (VISA / MC / AMEX)
        ↓
3. validatePago() ejecuta en tiempo real:
        ├── fmtOk:        ¿16 dígitos numéricos?
        ├── luhnOk:       ¿pasa algoritmo Luhn?
        ├── registeredOk: findCardByNum() → ¿está en cards[]?
        ├── statusOk:     ¿card.status === "activa"?
        ├── expOk:        ¿fecha válida y coincide con card.exp de la BD?
        ├── cvvOk:        ¿3-4 dígitos? (formato, no valor)
        ├── amtOk:        ¿monto entre $1.000 y card.balance?
        ├── nameOk:       ¿nombre >= 3 caracteres?
        └── nameMatchOk:  ¿alguna palabra coincide con card.name?
        ↓  (si todo OK → habilita botón "Procesar Pago")
4. procesarPago() — delay 1.8s simulando red
        ↓
5. Verifica cvv === card.cvv  (valor real)
        ├── SI → cards[idx].balance -= amount
        │         transactions[cardId].unshift({ status: "aprobado", ... })
        │         showReceiptApproved() → muestra recibo con nuevo saldo
        └── NO → transactions[cardId].unshift({ status: "rechazado", reason: "CVV incorrecto" })
                  showReceiptRejected() → muestra modal de rechazo
```

---

## Arquitectura del Sistema

Aunque el proyecto es un archivo HTML único, la lógica está organizada en capas con separación de responsabilidades:

| Capa | Responsabilidad | Implementación |
|------|----------------|----------------|
| **Data Layer** | Persistencia en memoria | Variables globales `cards[]` y `transactions{}` inicializadas con datos de prueba |
| **Service Layer** | Lógica de negocio y validaciones | `findCardByNum()`, `validatePago()`, `procesarPago()`, `luhn()` |
| **CRUD Layer** | Operaciones sobre la entidad | `createCard()`, `startEdit()`, `saveEdit()`, `deleteCard()` |
| **UI Layer** | Presentación e interactividad | `renderTable()`, `renderStats()`, `showTxnPanel()`, `showReceiptApproved/Rejected()` |
| **CSS Layer** | Estilos y tema visual | Variables CSS custom properties, Grid, Flexbox, responsive con media queries |

### Tecnologías

| Tecnología | Uso |
|-----------|-----|
| HTML5 | Estructura semántica de las dos pantallas y modales |
| CSS3 | Variables CSS, Grid, Flexbox, animaciones, tema oscuro bancario |
| JavaScript ES2021 | Lógica completa: validaciones, CRUD, gestión de estado, renderizado |
| Google Fonts | Tipografías Sora (UI) y DM Mono (números, código) |
| Sin frameworks | Aplicación vanilla pura — sin jQuery, React ni Vue |

---

## Convenciones del Repositorio

### Ramas

| Rama | Propósito |
|------|-----------|
| `main` | Rama principal — solo código aprobado y funcional |
| `feature/pantalla-pago` | Desarrollo del formulario de pago (Pantalla 1) |
| `feature/crud-tarjetas` | Desarrollo del CRUD de tarjetas (Pantalla 2) |
| `feature/historial-txn` | Panel de historial de transacciones por cliente |
| `feature/validaciones` | Validaciones avanzadas: Luhn, CVV, saldo, nombre |
| `fix/descuento-saldo` | Corrección: descuento de saldo al aprobar pago |
| `fix/cvv-visible` | Corrección: toggle ver/ocultar CVV en el CRUD |

### Convención de commits

```
feat: agregar validación de saldo disponible en formulario de pago
feat: implementar historial de transacciones por tarjeta en CRUD
feat: toggle ver/ocultar CVV en tabla y formularios
fix:  descuento correcto del saldo al aprobar transacción
fix:  verificación de tarjeta registrada antes de procesar pago
refactor: separar lógica de validación en función validatePago()
docs: actualizar README con nuevas funcionalidades v2
style: mejorar diseño del panel de transacciones
```

### Pull Requests

- Cada PR debe ser pequeño y enfocado en una sola funcionalidad o corrección
- Título: `[FEAT]`, `[FIX]`, `[DOCS]` o `[STYLE]` + descripción breve
- Debe incluir: descripción del cambio, captura de pantalla si aplica, reviewer asignado
- Todos los integrantes del equipo deben tener al menos un PR mergeado

---

*BancoPay v2.0 · Proyecto de Ingeniería de Software · 2025*
