# Spec: Inscripción de Participantes

## 1. Objetivo y Contexto
Permitir a los usuarios generar una cuenta en la plataforma y registrarse de manera autónoma en los eventos académicos disponibles. Este módulo es fundamental para controlar la asistencia y garantizar que no se excedan las capacidades físicas o virtuales de cada evento.

## 2. Historias de Usuario y Criterios de Aceptación
**Historia de Usuario:**
Como participante registrado, quiero inscribirme a un evento académico publicado para asegurar mi lugar y poder asistir.

**Criterios de Aceptación:**
- Si un participante intenta inscribirse a un evento cuya "fecha límite para inscripción" ya ha pasado, el sistema debe rechazar la operación y mostrar un mensaje de error claro.
- Si el evento ya alcanzó su cupo máximo de inscriptos, no se debe permitir ninguna nueva inscripción.
- Un participante no puede inscribirse dos veces al mismo evento (validación de duplicidad).

## 3. Requisitos Funcionales y Reglas de Negocio
- [cite_start]Los participantes deben generar un usuario en la plataforma previamente para poder hacer la inscripción a un evento[cite: 43].
- El sistema debe registrar la fecha y hora exacta en la que el usuario realiza la inscripción.
- [cite_start]La lógica de negocio debe respetar los cupos máximos y las fechas límite establecidas por el organizador en el módulo de Gestión de Eventos[cite: 46].
- El estado por defecto de una nueva inscripción exitosa debe ser "Confirmada".

## 4. Restricciones técnicas específicas de este módulo
- **Backend:** C# con .NET 8. Las validaciones de cupo y fecha límite deben realizarse en el servidor, no solo en el cliente, para evitar manipulaciones.
- **Persistencia:** Entity Framework Core sobre PostgreSQL.
- **Transacciones:** La verificación de cupo disponible y la posterior inserción del registro de inscripción deben ejecutarse dentro de una transacción de base de datos para evitar condiciones de carrera (race conditions) si múltiples usuarios se inscriben al mismo tiempo.

## 5. Modelo de datos de este módulo
**Entidad `Inscripcion`**
- `Id` (Guid, Primary Key)
- `UsuarioId` (Guid, Foreign Key hacia la tabla de Usuarios)
- `EventoId` (Guid, Foreign Key hacia la tabla de Eventos)
- `FechaAlta` (DateTime, almacenada en formato UTC)
- `Estado` (String o Enum: Confirmada, Cancelada)

## 6. Plan de Tareas
- **Tarea 1:** Crear la entidad `Inscripcion` con sus relaciones (Foreign Keys) hacia Usuario y Evento, y generar la migración correspondiente en Entity Framework.
- **Tarea 2:** Desarrollar el servicio de backend con la lógica transaccional para validar fecha límite, cupos y evitar duplicados.
- **Tarea 3:** Crear el endpoint `POST /api/inscripciones`.
- **Tarea 4:** Implementar el botón y el formulario de inscripción en el frontend utilizando componentes de Blazor WebAssembly.

## 7. Estrategia de Verificación
- **Pruebas Unitarias:** Crear tests para el servicio de backend asegurando que se lance una excepción si se intenta inscribir a un evento lleno o vencido.
- **Pruebas de Integración:** Verificar la correcta persistencia del registro en PostgreSQL y el código HTTP 200 de retorno en la API bajo un escenario de éxito.
