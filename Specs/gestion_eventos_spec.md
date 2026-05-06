1. Objetivo y Contexto: Permitir a los usuarios con rol de "organizador" dar de alta, modificar y publicar eventos académicos (cursos, congresos, etc.) para que sean visibles al público general.  

2. Historias de Usuario y Criterios de Aceptación:

    - Historia: Como organizador quiero crear un nuevo evento en el sistema para publicarlo en el listado público.  

    - Criterios:

    	- Si el organizador intenta crear un evento sin definir una fecha de realización, el sistema debe rechazar la operación.  

	    - El evento debe permitir configurar un cupo mínimo y máximo de participantes.  

3. Requisitos Funcionales y Reglas de Negocio:

    - El listado de eventos debe ser público.  

    - Se debe poder filtrar eventos a futuro y eventos que ya han pasado.  

    - Se debe establecer una fecha límite para la inscripción.  

4. Restricciones técnicas específicas:

    - Forzado: Usar Entity Framework Core para la persistencia en PostgreSQL.

5. Modelo de datos de este módulo:

    - Entidad Evento: ID (PK), Titulo, Descripcion, Tipo (Curso, Jornada, Congreso), FechaRealizacion, FechaLimiteInscripcion, CupoMinimo, CupoMaximo.

6. Plan de Tareas (para ejecución ordenada del agente):

    - Tarea 1: Crear el modelo Evento y aplicar las migraciones a la base de datos.  

    - Tarea 2: Crear los endpoints CRUD en la API para la gestión del evento.  

    - Tarea 3: Implementar la interfaz de usuario con los filtros de fecha.  

7. Estrategia de Verificación:

    - Ejecutar pruebas unitarias verificando que no se puedan insertar eventos con cupo negativo.  

    - Verificar que el endpoint público liste únicamente los eventos publicados.
