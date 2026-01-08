# Reto Técnico: Sistema de Gestión de Comprobantes

## Información General

| Campo | Detalle |
|-------|---------|
| **Duración** | 7 horas (9:00 AM - 4:00 PM, incluye almuerzo) |
| **Modalidad** | Desarrollo + Defensa de código |
| **Stack** | C# .NET 8, PostgreSQL, React o Blazor |
| **Herramientas** | Puedes usar cualquier recurso (IA, documentación, etc.) |

---

## Contexto del Negocio

Eres parte del equipo de desarrollo de una empresa de software contable en Perú. Necesitas construir un microservicio para gestionar comprobantes electrónicos (facturas y boletas) que cumpla con las regulaciones de SUNAT.

---

## Requerimientos

### Backend - API REST (2 horas)

Construir una API con los siguientes endpoints:

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| `POST` | `/api/comprobantes` | Crear un nuevo comprobante |
| `GET` | `/api/comprobantes` | Listar comprobantes con paginación y filtros |
| `GET` | `/api/comprobantes/{id}` | Obtener detalle de un comprobante |
| `PUT` | `/api/comprobantes/{id}/anular` | Anular un comprobante |

### Modelo de Datos

```
Comprobante
├── Id (GUID)
├── Tipo (Factura | Boleta)
├── Serie (string, 4 caracteres)
├── Numero (int)
├── FechaEmision (DateTime)
├── RucEmisor (string, 11 dígitos)
├── RazonSocialEmisor (string)
├── RucReceptor (string, 11 dígitos)
├── RazonSocialReceptor (string)
├── SubTotal (decimal)
├── IGV (decimal, calculado)
├── Total (decimal, calculado)
├── Estado (Emitido | Anulado)
└── Items[]
    ├── Descripcion (string)
    ├── Cantidad (int)
    ├── PrecioUnitario (decimal)
    └── Subtotal (decimal, calculado)
```

### Reglas de Negocio

1. **RUC**: Debe tener exactamente 11 dígitos numéricos
2. **IGV**: Se calcula como el 18% del subtotal
3. **Total**: SubTotal + IGV
4. **Serie**: 
   - Facturas: Formato `F###` (ej: F001, F002)
   - Boletas: Formato `B###` (ej: B001, B002)
5. **Anulación**: No se puede anular un comprobante ya anulado
6. **Número**: Autoincremental por serie

### Filtros para el Listado

- `fechaDesde` y `fechaHasta`: Rango de fechas de emisión
- `tipo`: Filtrar por Factura o Boleta
- `rucReceptor`: Búsqueda por RUC del cliente
- `estado`: Filtrar por Emitido o Anulado
- `page` y `pageSize`: Paginación

---

### Frontend (1 hora)

Construir una interfaz simple con:

1. **Listado de comprobantes**
   - Tabla con los datos principales
   - Filtros básicos (al menos por tipo y estado)
   - Paginación

2. **Formulario de creación**
   - Campos del comprobante
   - Agregar/eliminar items dinámicamente
   - Cálculo automático de subtotales, IGV y total

3. **Acción de anulación**
   - Botón en cada fila o en el detalle
   - Confirmación antes de anular

---

### Requerimientos Adicionales

- Tests unitarios para las validaciones de negocio
- Documentación Swagger/OpenAPI
- Manejo de errores con ProblemDetails (RFC 7807)
- Logging estructurado
- Docker Compose funcional

---

## Configuración

1. **Crea un repositorio en tu cuenta personal de GitHub**
   - Nombre sugerido: `reto-fullstack-contasiscorp`
   - Puede ser público o privado

2. **Agrega como colaborador a**: `alejandro.xux`
   - Ve a Settings → Collaborators → Add people
   - Esto es **obligatorio** para que podamos revisar tu código

3. **Configura tu entorno de desarrollo como prefieras**
   - Necesitarás: .NET 8 SDK, PostgreSQL, y Node.js (si usas React)

4. **Haz commits frecuentes** con mensajes descriptivos

---

## Estructura Sugerida del Repositorio

```
tu-repositorio/
├── README.md                   # Instrucciones para ejecutar tu solución
├── docker-compose.yml          # PostgreSQL y servicios necesarios
├── src/
│   ├── Api/
│   │   ├── Controllers/
│   │   ├── DTOs/
│   │   ├── Middleware/         # Manejo de errores (ProblemDetails)
│   │   └── Program.cs          # Configuración de Swagger y logging
│   ├── Application/
│   │   ├── Commands/
│   │   ├── Queries/
│   │   └── Validators/
│   ├── Domain/
│   │   ├── Entities/
│   │   ├── Enums/
│   │   └── Exceptions/
│   ├── Infrastructure/
│   │   ├── Data/
│   │   └── Repositories/
│   └── Web/
│       └── (React o Blazor)
└── tests/
    └── UnitTests/              # Tests de validaciones de negocio
```

---

## Criterios de Evaluación

| Criterio | Peso | Descripción |
|----------|------|-------------|
| **Funcionalidad** | 25% | Los endpoints funcionan correctamente |
| **Arquitectura** | 25% | Separación de responsabilidades, Clean Architecture |
| **Código C#** | 20% | Uso idiomático del lenguaje, async/await, manejo de errores |
| **Frontend** | 15% | Interfaz funcional y usable |
| **Video de defensa** | 15% | Claridad al explicar, dominio del código, respuestas coherentes |

---

## Importante: Defensa del Código (Video Loom)

Después de completar el desarrollo, debes grabar un video explicando tu solución.

### Instrucciones del Video

1. **Graba tu pantalla usando [Loom](https://www.loom.com/)** (cuenta gratuita disponible)
2. **Duración máxima: 30 minutos**
3. **Envía el link del video** junto con el link de tu repositorio

### Contenido del Video

Tu video debe cubrir:

1. **Demo funcional** (~5 min)
   - Muestra la aplicación funcionando
   - Crea un comprobante, lista, filtra y anula

2. **Explicación de arquitectura** (~10 min)
   - Recorre la estructura de tu proyecto
   - Explica la separación de capas y responsabilidades
   - Justifica tus decisiones técnicas

3. **Code walkthrough** (~10 min)
   - Muestra el flujo de una request (controller → service → repository)
   - Explica las validaciones de negocio
   - Muestra cómo configuraste EF Core

4. **Responde estas preguntas en el video** (~5 min)
   - ¿Por qué elegiste esta estructura de carpetas/capas?
   - ¿Qué mejorarías si tuvieras más tiempo?
   - ¿Cómo manejarías concurrencia si dos usuarios anulan el mismo comprobante simultáneamente?

---

## Agenda

| Hora | Actividad |
|------|-----------|
| 9:00 - 13:00 | **Desarrollo** |
| 13:00 - 14:00 | Almuerzo |
| 14:00 - 16:00 | **Grabar video en Loom** y enviar enlaces |

### Entregables al finalizar (máximo 4:00 PM)

Enviar a:
- **m.cuzcano@contasiscorp.com**
- **a.mayta@finacontcorp.com**

| Entregable | Requisito |
|------------|-----------|
| **Repositorio GitHub** | Colaborador `alejandro.xux` agregado |
| **Video Loom** | Máximo 30 minutos, cubriendo los puntos indicados |

**Sin video = evaluación incompleta**

---

## Tips

1. **Prioriza funcionalidad sobre perfección**: Es mejor tener algo funcionando que código perfecto incompleto
2. **Commits frecuentes**: Haz commits pequeños con mensajes descriptivos
3. **Entiende lo que escribes**: Vas a tener que explicarlo y modificarlo
4. **Pregunta si tienes dudas**: Sobre requerimientos de negocio, no sobre implementación

---

¡Buena suerte!
