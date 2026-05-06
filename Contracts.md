# Contratos y Estándares
- Formato de fechas: Todas las fechas del sistema deben manejarse y almacenarse en formato ISO 8601.
- Respuestas API: Toda respuesta exitosa de la API debe devolver un código HTTP 200 y un JSON estandarizado.
- Manejo de Errores: Los errores de validación de negocio deben devolver HTTP 400 con un mensaje claro en español.
- Tipado: Se requiere tipado estricto en todos los modelos de datos; no permitir campos nulos en las bases de datos a menos que se especifique en la spec.
