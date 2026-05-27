```yaml
requisitos:
  prd_documento: |
    #SENA Gestor de Horarios V6 PRD

    El producto es un MVP para gestionar los cronogramas de capacitación del SENA con Go,
    Reaccionar, MongoDB, Docker y Docker Compose. El sistema es compatible con el aula,
    Gestión de grupos de estudiantes, instructores, horarios y observaciones. el objetivo
    El usuario es un coordinador de horarios que necesita una herramienta operativa simple para
    crear registros de catálogo, crear cronogramas con rangos de tiempo validados y
    registrar observaciones para seguimiento.

    Este MVP reduce intencionalmente el alcance funcional, no la calidad técnica.
    El espacio de trabajo generado debe mantener la base de datos, el backend y el frontend como
    componentes separados. El esquema de base de datos/init/index/propiedad de la semilla pertenece a
    el componente de base de datos. El dominio/núcleo backend debe permanecer independiente de
    MongoDB y detalles de transporte; Los documentos de persistencia y los mapeadores pertenecen a
    repositorio/infraestructura. El frontend debe utilizar una capa de servicio/API y debe
    no convertirse en un componente monolítico de la aplicación.

    Identificadores técnicos, código fuente, contratos API, diagramas, contenedor.
    Los nombres y objetos de la base de datos deben usar inglés. El español está permitido sólo para
    copia de cara al usuario y documentación funcional. La carrera es una prueba de fuego de
    el sistema operativo Agentic SDLC, por lo que se deben generar pruebas de control de calidad, AppSec y versión
    como artefactos reales antes de su finalización.
  requisitos_funcionales:
    - Crear y enumerar registros de aula con código, nombre, ubicación y capacidad.
    - Cree y enumere registros de grupos de estudiantes con código, nombre del programa y recuento de alumnos.
    - Crear y enumerar registros de instructores con tipo de personal o contratista.
    - Crear y enumerar registros de horarios vinculados al aula, grupo de estudiantes e instructor.
    - Validar el día del cronograma, la hora de inicio y la hora de finalización antes de la persistencia.
    - Crear y enumerar observaciones vinculadas a un cronograma.
    - Exponer el estado del backend y los puntos finales de la API CRUD para entidades MVP.
    - Proporcionar pantallas de interfaz para enumerar y crear entidades MVP a través de un cliente API.
  requisitos_no_funcionales:
    arquitectura: "la base de datos, el backend y el frontend son componentes independientes del espacio de trabajo con sus propios límites de contenedores".
    mantenibilidad: "ningún archivo fuente supera las 300 líneas; ningún archivo principal, aplicación, página, repositorio, controlador o CSS monolítico".
    idioma: "los identificadores técnicos, el código fuente, los contratos API y los objetos de la base de datos usan inglés; la copia de la interfaz de usuario puede estar en español".
    data: "Las colecciones y atributos de MongoDB usan nombres en inglés singular lower_snake_case cuando corresponda".
    domain_boundary: "el código de dominio/núcleo no contiene metadatos de persistencia o transporte; los modelos y mapeadores de persistencia viven en el repositorio/infraestructura".
    testing: "el backend y el frontend incluyen pruebas reales con comandos ejecutables".
    seguridad: "el backend desinfecta los errores internos, restringe CORS, declara tiempos de espera y evita el almacenamiento secreto del lado del cliente".
    release: "Docker Compose, la preparación para el lanzamiento, el plan de implementación y el plan de reversión se elaboran antes de su finalización".
  historias_de_usuario:
    - identificación: EE.UU.-001
      como: "coordinador de programación"
      i_want: "crear y listar aulas"
      so_that: "los horarios se pueden asignar a entornos físicos"
    - identificación: US-002
      como: "coordinador de programación"
      i_want: "crear y enumerar grupos de estudiantes"
      so_that: "los grupos de entrenamiento se pueden programar con precisión"
    - identificación: US-003
      como: "coordinador de programación"
      i_want: "crear y enumerar instructores con tipo de personal o contratista"
      so_that: "las tareas docentes distinguen la relación laboral"
    - identificación: US-004
      como: "coordinador de programación"
      i_want: "crear horarios con aula, grupo, instructor, día y rango horario"
      so_that: "las sesiones de formación se pueden planificar de forma coherente"
    - identificación: US-005
      como: "coordinador de programación"
      i_want: "adjuntar observaciones a los cronogramas"
      so_that: "las notas de seguimiento permanecen conectadas al registro del cronograma"
    - identificación: US-006
      como: "coordinador de programación"
      i_want: "ver errores de validación para campos faltantes o rangos de tiempo no válidos"
      so_that: "los malos horarios no persisten"
  criterios_de_aceptación:
    - "Docker Compose puede iniciar bases de datos, backend y frontend como servicios distintos".
    - "El inicio de MongoDB, los índices y los artefactos semilla se encuentran en la base de datos, no en el backend".
    - "El backend expone puntos finales CRUD y de salud para el aula, el grupo de estudiantes, el instructor, el horario y la observación".
    - "Los archivos de dominio/núcleo de backend no contienen anotaciones bson, gorm, db, sql, ORM/ODM ni importaciones de controladores de base de datos".
    - "La interfaz enumera y crea registros MVP a través de un cliente API con manejo de tiempo de espera".
    - "El backend y el frontend incluyen pruebas reales y comandos de verificación declarados".
    - "A09 produce evidencia de control de calidad con comandos de prueba y clasificación de defectos".
    - "A12 produce evidencia de AppSec con OWASP/amenaza/control/cobertura de riesgo y evidencia de tiempo de ejecución".
    - "A11 produce evidencia de preparación para el lanzamiento, implementación y reversión".
  alcance_límites:
    en_alcance:
      - "gestión del aula"
      - "gestión de grupos de estudiantes"
      - "gestión de instructores"
      - "gestión de horarios"
      - "programar observaciones"
      - "Tiempo de ejecución local de Docker Compose"
      - "QA, AppSec y evidencia de liberación"
    fuera_de_alcance:
      - "autenticación y gestión de roles"
      - "calendario de arrastrar y soltar"
      - "informes masivos"
      - "importar/exportar"
      - "implementación de la nube de producción"
  partes interesadas:
    - "coordinador de programación: usuario operativo principal"
    - “Coordinación académica del SENA: empresario empresario”
    - "revisor técnico: valida la arquitectura, el control de calidad, la seguridad y la preparación para el lanzamiento"
  puntuación de calidad: 0,91
```