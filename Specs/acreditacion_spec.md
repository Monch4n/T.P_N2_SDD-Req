# Spec: Acreditación de Participantes

## 1. Objetivo y Contexto
[cite_start]Permitir al personal organizador del evento registrar la asistencia real de los participantes el día de la realización. Esto es clave para distinguir a quienes solo se inscribieron de quienes efectivamente asistieron, habilitando posteriormente la emisión de certificados.

## 2. Historias de Usuario y Criterios de Aceptación
**Historia de Usuario:**
Como personal del evento, quiero buscar a un participante inscrito y registrar su acreditación (asistencia) para habilitar su participación formal.

**Criterios de Aceptación:**
- El sistema debe permitir buscar inscriptos por DNI o Nombre/Apellido.
- Solo se puede acreditar a un participante cuya inscripción previa esté en estado "Confirmada".
- Si se intenta acreditar a una persona que no está en la lista de inscriptos, el sistema debe rechazar la operación y ofrecer la opción de "Inscripción tardía" (si el evento lo permite).

## 3. Requisitos Funcionales y Reglas de Negocio
- La acreditación debe registrar la hora exacta de ingreso del participante.
- Se debe poder visualizar en tiempo real el porcentaje de acreditados vs. inscritos.
- El personal debe poder deshacer una acreditación en caso de error humano.

## 4. Restricciones técnicas específicas de este módulo
- **Backend:** C# con .NET 8.
- **Frontend:** Blazor WebAssembly. [cite_start]La vista de acreditación debe ser rápida y responsiva, pensada para usarse desde dispositivos móviles o tablets en la puerta del evento[cite: 34].
- **Persistencia:** Entity Framework Core sobre PostgreSQL.

## 5. Modelo de datos de este módulo
Esta funcionalidad extiende la entidad existente `Inscripcion`, agregando/modificando campos, o crea un registro asociado:
**Entidad `Acreditacion`**
- `Id` (Guid, Primary Key)
- `InscripcionId` (Guid, Foreign Key)
- `FechaHoraIngreso` (DateTime)
- `AcreditadoPorUsuarioId` (Guid, Foreign Key al organizador/personal)

## 6. Plan de Tareas
- **Tarea 1:** Crear el modelo `Acreditacion` y aplicar la migración a la base de datos.
- **Tarea 2:** Desarrollar el endpoint `POST /api/acreditaciones` para registrar la asistencia.
- **Tarea 3:** Desarrollar un endpoint de búsqueda rápida (filtrado in-memory o consulta optimizada en BD) para buscar participantes.
- **Tarea 4:** Implementar la interfaz de usuario en Blazor orientada a la rápida lectura de datos.

## 7. Estrategia de Verificación
- **Pruebas Unitarias:** Comprobar que no se pueda registrar dos veces la misma acreditación para un mismo `InscripcionId`.
- **Pruebas de Integración:** Verificar que la búsqueda devuelva correctamente los datos del participante en menos de 200ms.
