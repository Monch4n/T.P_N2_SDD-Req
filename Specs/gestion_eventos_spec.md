# Spec: Gestión de Eventos

## 1. Objetivo y Contexto
[cite_start]El objetivo de este módulo es permitir a los usuarios con rol de organizador crear, administrar y publicar eventos académicos (tales como cursos, jornadas, congresos y charlas)[cite: 33]. Esta es la funcionalidad central (core) del sistema, ya que sin eventos creados no puede haber inscripciones ni emisión de certificados posteriores.

## 2. Historias de Usuario y Criterios de Aceptación
**Historia de Usuario:**
Como organizador registrado en la plataforma, quiero crear y configurar un nuevo evento académico para publicarlo y habilitar la inscripción de participantes.

**Criterios de Aceptación:**
- [cite_start]El sistema debe requerir obligatoriamente el ingreso de una fecha de realización y un tipo de evento; de lo contrario, debe rechazar la creación[cite: 44].
- [cite_start]El organizador debe poder establecer un cupo mínimo y un cupo máximo de asistentes permitidos para el evento[cite: 46].
- [cite_start]Se debe poder configurar una fecha y hora límite estricta para la inscripción[cite: 46].
- Si el cupo máximo ingresado es menor al cupo mínimo, el sistema debe mostrar un error de validación.

## 3. Requisitos Funcionales y Reglas de Negocio
- [cite_start]El listado de eventos activos y publicados debe ser de acceso público (no requiere inicio de sesión para ser visualizado)[cite: 44].
- [cite_start]En el listado público, se debe proveer un filtro que permita a los usuarios visualizar únicamente los eventos a futuro o revisar el histórico de los eventos que ya han pasado[cite: 45].
- Solo el usuario creador (organizador) o un administrador del sistema puede modificar los detalles de un evento una vez creado.

## 4. Restricciones técnicas específicas de este módulo
- **Backend:** Desarrollado en C# con .NET 8.
- **Persistencia:** Entity Framework Core conectado a PostgreSQL.
- **Validaciones:** Utilizar Data Annotations o FluentValidation en los DTOs para asegurar que los datos ingresados desde el cliente sean correctos antes de procesar la lógica de negocio.

## 5. Modelo de datos de este módulo
**Entidad `Evento`**
- `Id` (Guid, Primary Key)
- `Titulo` (String, MaxLength 150)
- `Descripcion` (String)
- `TipoEvento` (String o Enum: Curso, Jornada, Congreso, Charla)
- `FechaRealizacion` (DateTime)
- `FechaLimiteInscripcion` (DateTime)
- `CupoMinimo` (Int, default 0)
- `CupoMaximo` (Int)
- `OrganizadorId` (Guid, Foreign Key)
- `Estado` (Enum: Borrador, Publicado, Cancelado, Finalizado)

## 6. Plan de Tareas
- **Tarea 1:** Definir el modelo `Evento` en el backend y generar la migración para actualizar el esquema de la base de datos.
- **Tarea 2:** Desarrollar los endpoints de la API REST (GET, POST, PUT, DELETE) para la gestión completa del recurso.
- **Tarea 3:** Implementar un endpoint específico para el listado público que acepte parámetros de consulta (query parameters) para filtrar por fechas (futuros/pasados).
- **Tarea 4:** Construir los componentes de interfaz en Blazor para el formulario de alta/edición y para la grilla de visualización pública con sus respectivos filtros.

## 7. Estrategia de Verificación
- **Pruebas Unitarias:** Validar que la lógica de dominio rechace la creación de un evento si la `FechaLimiteInscripcion` es posterior a la `FechaRealizacion`.
- **Pruebas de Integración:** Verificar que el endpoint de búsqueda devuelva correctamente los eventos filtrados según el parámetro de fecha ingresado, y que devuelva un código HTTP 200.
