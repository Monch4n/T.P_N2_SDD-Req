# Spec: Generación de Certificados

## 1. Objetivo y Contexto
[cite_start]Proveer un mecanismo automatizado para emitir certificados digitales (de asistencia, aprobación, o autor/expositor) a los participantes una vez finalizado el evento. 

## 2. Historias de Usuario y Criterios de Aceptación
**Historia de Usuario:**
Como participante acreditado, quiero generar y descargar mi certificado en formato PDF para poder adjuntarlo a mi currículum.

**Criterios de Aceptación:**
- El sistema solo debe permitir la descarga del certificado si el usuario tiene un registro de `Acreditacion` válido (asistencia confirmada).
- [cite_start]El certificado debe incluir el nombre del participante, nombre del evento, fecha, y tipo de participación (ej. "Asistente" o "Expositor").
- Cada certificado generado debe contar con un código identificador único (Hash o UUID) para validar su autenticidad.

## 3. Requisitos Funcionales y Reglas de Negocio
- Los certificados solo pueden ser generados si el evento se encuentra en estado "Finalizado".
- El organizador puede configurar plantillas base para los diferentes tipos de certificados.
- El sistema debe exportar el certificado directamente en formato PDF.

## 4. Restricciones técnicas específicas de este módulo
- **Backend:** C# con .NET 8.
- **Librería de PDF:** Forzado: Utilizar la librería QuestPDF (o iText7) para la maquetación y generación del documento PDF en el servidor.
- **Almacenamiento:** No guardar los archivos PDF en la base de datos PostgreSQL; guardar únicamente el registro de emisión y generar el PDF "al vuelo" (on-the-fly) cuando sea solicitado, o usar un storage externo.

## 5. Modelo de datos de este módulo
**Entidad `CertificadoEmitido`**
- `Id` (Guid, Primary Key)
- `AcreditacionId` (Guid, Foreign Key)
- [cite_start]`TipoCertificado` (Enum: Asistencia, Aprobacion, Expositor) 
- `CodigoVerificacion` (String, Unique)
- `FechaEmision` (DateTime)

## 6. Plan de Tareas
- **Tarea 1:** Configurar la librería de generación de PDFs en el proyecto backend.
- **Tarea 2:** Crear la entidad `CertificadoEmitido` y su migración correspondiente.
- **Tarea 3:** Desarrollar el servicio que toma los datos del evento y del usuario, los inyecta en la plantilla y devuelve un stream del PDF.
- **Tarea 4:** Implementar el endpoint `GET /api/certificados/{codigo}/descargar`.

## 7. Estrategia de Verificación
- **Pruebas Unitarias:** Validar que el servicio arroje error si un participante sin acreditación intenta generar el documento.
- **Pruebas de Integración:** Verificar que el endpoint de descarga devuelva el `Content-Type` correcto (`application/pdf`).
