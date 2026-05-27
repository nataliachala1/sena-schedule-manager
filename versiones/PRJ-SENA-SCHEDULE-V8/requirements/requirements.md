```yaml
requisitos:
  prd:
    título: "SENA Schedule Manager v8"
    versión: "v8"
    contexto: "Producto de programación interna para operaciones de centros de capacitación, gestión de entornos de aprendizaje, cronogramas, grupos de capacitación, instructores, tipos de contratos y observaciones operativas. El objetivo es MVP funcional y aplicación mecánica de arquitectura, calidad, seguridad y preparación para el lanzamiento".
    pila_requerida:
      - "Servicio de fondo: Ir"
      - "Frontal: Reaccionar"
      - "Base de datos: MongoDB"
      - "Tiempo de ejecución: Docker y Docker Compose"
    reglas_de_ingeniería_no_negociables:
      - "MVP reduce el alcance funcional, nunca la arquitectura, la seguridad, las pruebas, la observabilidad o los estándares de lanzamiento".
      - "El espacio de trabajo debe dividirse en tres proyectos independientes: base de datos/, backend/ y frontend/."
      - "Cada componente debe poseer su límite de contenedor y Dockerfile".
      - "La inicialización de la base de datos, los índices, la validación, los datos iniciales o las migraciones deben residir en la base de datos/, nunca dentro del backend/".
      - "El código técnico, los identificadores de bases de datos, los contratos de API, los nombres de archivos, las pruebas, los comentarios, los diagramas y los contratos de arquitectura deben estar en inglés".
      - "Las etiquetas de cara al usuario y la documentación del producto pueden estar en español".
      - "Los nombres y atributos de las colecciones de bases de datos deben estar en inglés, ser singulares y coherentes".
      - "El backend debe respetar la inversión de dependencias y limpiar los límites de la arquitectura".
      - "La interfaz debe evitar páginas/componentes monolíticos y debe mantener a los clientes API fuera de los componentes".
      - "El control de calidad, AppSec y las pruebas de publicación deben citar comandos, archivos, códigos de salida y artefactos reales".
    alcance_funcional:
      - "Gestionar entornos de aprendizaje".
      - "Gestionar horarios".
      - "Gestionar grupos de entrenamiento".
      - "Gestionar instructores".
      - “Distinguir a los instructores de planta y a los instructores contratistas”.
      - “Registrar observaciones operativas vinculadas a horarios o instructores”.
      - "Exponer los flujos de trabajo CRUD básicos que necesitan los coordinadores".
    roles_usuario:
      - "Coordinador académico: gestiona horarios, ambientes, grupos, instructores y observaciones".
      - "Instructor: visualiza horarios asignados y observaciones relacionadas".
      - "Administrador: valida los datos operativos y la preparación del sistema".
  requisitos_funcionales:
    - identificación: "FR1"
      descripción: "El sistema debe permitir a los coordinadores crear, actualizar, enumerar y desactivar entornos de aprendizaje."
    - identificación: "FR2"
      descripción: "El sistema debe permitir a los coordinadores crear, actualizar, enumerar y desactivar grupos de capacitación."
    - identificación: "FR3"
      descripción: "El sistema debe permitir a los coordinadores crear, actualizar, enumerar y desactivar instructores."
    - identificación: "FR4"
      descripción: "El sistema debe clasificar a los instructores por tipo de contrato: personal o contratista."
    - identificación: "FR5"
      descripción: "El sistema debe permitir a los coordinadores crear y actualizar cronogramas vinculando el entorno, el grupo de capacitación, el instructor, el día, el rango de tiempo y las notas."
    - identificación: "FR6"
      descripción: "El sistema debe evitar horarios obviamente no válidos, como la falta de un instructor, un entorno faltante o un rango de tiempo no válido."
    - identificación: "FR7"
      descripción: "El sistema debe permitir que las observaciones se registren y asocien con un horario, instructor o contexto operativo."
    - identificación: "FR8"
      descripción: "La interfaz debe proporcionar flujos utilizables para las entidades base sin depender de un comportamiento simulado."
    - identificación: "FR9"
      descripción: "El backend debe exponer las API HTTP con validación, normalización de errores y abstracciones del repositorio."
    - identificación: "FR10"
      descripción: "El proyecto de base de datos debe definir colecciones, índices, reglas de validación y datos iniciales de MongoDB independientemente del inicio del backend."
  requisitos_no_funcionales:
    - identificación: "NFR1"
      descripción: "La compilación debe ser reproducible desde una caja limpia".
    - identificación: "NFR2"
      descripción: "Las pruebas de backend deben ejecutarse mediante un comando documentado."
    - identificación: "NFR3"
      descripción: "Las pruebas de frontend y la compilación de producción deben ejecutarse mediante comandos documentados."
    - identificación: "NFR4"
      descripción: "Docker Compose debe iniciar la base de datos, el backend y el frontend como servicios separados".
    - identificación: "NFR5"
      descripción: "Los servicios deben incluir controles de salud o controles de preparación equivalentes".
    - identificación: "NFR6"
      descripción: "El servidor HTTP backend debe utilizar tiempos de espera."
    - identificación: "NFR7"
      descripción: "Los clientes API deben utilizar tiempos de espera."
    - identificación: "NFR8"
      descripción: "CORS no debe estar abierto de forma predeterminada."
    - identificación: "NFR9"
      descripción: "Las respuestas de error no deben filtrar errores internos sin procesar."
    - identificación: "NFR10"
      descripción: "La evidencia de AppSec debe hacer referencia a controles y archivos concretos".
    - identificación: "NFR11"
      descripción: "La preparación para el lanzamiento debe incluir comandos de implementación y reversión".
  historias_de_usuario:
    - identificación: "US1"
      as_a: "Coordinador académico"
      i_want_to: "crear, actualizar, enumerar y desactivar entornos de aprendizaje"
      so_that: "Puedo gestionar los espacios físicos disponibles para entrenar."
      criterios_de_aceptación:
        - "Puedo crear un nuevo entorno de aprendizaje con un nombre y capacidad".
        - "Puedo modificar detalles de un entorno de aprendizaje existente".
        - "Puedo ver una lista de todos los entornos de aprendizaje".
        - "Puedo desactivar un entorno, haciéndolo no disponible para nuevos horarios".
    - identificación: "US2"
      as_a: "Coordinador académico"
      i_want_to: "crear, actualizar, listar y desactivar grupos de entrenamiento"
      so_that: "Puedo organizar a los estudiantes en grupos distintos para la programación."
      criterios_de_aceptación:
        - "Puedo crear un nuevo grupo de entrenamiento con un nombre y una descripción".
        - "Puedo modificar detalles de un grupo de entrenamiento existente".
        - "Puedo ver una lista de todos los grupos de entrenamiento".
        - "Puedo desactivar un grupo, haciéndolo no disponible para nuevos horarios".
    - identificación: "US3"
      as_a: "Coordinador académico"
      i_want_to: "crear, actualizar, enumerar y desactivar instructores y clasificarlos por tipo de contrato"
      so_that: "Puedo gestionar el personal docente y su situación laboral."
      criterios_de_aceptación:
        - "Puedo crear un nuevo registro de instructor con nombre, contacto y tipo de contrato (personal/contratista)".
        - "Puedo modificar detalles de un instructor existente."
        - "Puedo ver una lista de todos los instructores, filtrando por tipo de contrato".
        - "Puedo desactivar a un instructor, haciéndolo no disponible para nuevos horarios".
    - identificación: "US4"
      as_a: "Coordinador académico"
      i_want_to: "crear y actualizar horarios vinculando el entorno, el grupo de capacitación, el instructor, el día, el rango de tiempo y las notas"
      so_that: "Puedo asignar recursos y horarios para actividades de capacitación."
      criterios_de_aceptación:
        - "Puedo crear un horario seleccionando un entorno, grupo de entrenamiento, instructor, día y rango de tiempo, agregando notas opcionales".
        - "Puedo actualizar los detalles de un horario existente".
        - "El sistema impide crear un horario sin un instructor seleccionado."
        - "El sistema impide crear un cronograma sin un ambiente seleccionado".
        - "El sistema impide crear un cronograma con un rango de tiempo no válido (por ejemplo, finalizar antes de comenzar)".
    - identificación: "US5"
      as_a: "Coordinador académico"
      i_want_to: "registrar observaciones operativas vinculadas a horarios, instructores o contexto operativo"
      so_that: "Puedo documentar eventos o asuntos importantes relacionados con las operaciones o el personal."
      criterios_de_aceptación:
        - "Puedo crear una observación y vincularla a un horario específico".
        - "Puedo crear una observación y vincularla a un instructor específico".
        - "Puedo crear una observación general no vinculada a un horario o instructor específico".
        - "Una observación incluye una descripción, una marca de tiempo y enlaces opcionales".
    - identificación: "US6"
      as_a: "Instructor"
      i_want_to: "ver mis horarios asignados y observaciones relacionadas"
      so_that: "Puedo mantenerme informado sobre mis tareas docentes y cualquier comentario relevante."
      criterios_de_aceptación:
        - "Puedo iniciar sesión y ver una lista de horarios asignados a mí."
        - "Para cada horario, puedo ver sus detalles (ambiente, grupo, hora)".
        - "Puedo ver cualquier observación específicamente vinculada a mis horarios o mi perfil de instructor".
    - identificación: "US7"
      as_a: "Administrador"
      i_want_to: "validar datos operativos y preparación del sistema"
      so_that: "Puedo asegurar que el sistema sea estable, seguro y esté listo para usar."
      criterios_de_aceptación:
        - "Puedo confirmar que todos los servicios se inician correctamente a través de Docker Compose".
        - "Puedo acceder a puntos finales de control de estado para cada servicio".
        - "Puedo revisar los registros en busca de errores críticos".
        - "Puedo verificar que los controles de seguridad (por ejemplo, CORS) estén configurados correctamente".
  criterios_de_aceptación:
    - "El espacio de trabajo generado contiene base de datos/, backend/ y frontend/".
    - "la base de datos/ contiene artefactos de inicialización de MongoDB, índices, validación y datos semilla usando nombres singulares en inglés".
    - "backend/contiene código Go organizado por dominio, aplicación/casos de uso, transporte HTTP, interfaces/implementaciones del repositorio y configuración".
    - "frontend/contiene código React organizado por composición de aplicación, características, componentes, servicios, enlaces y pruebas".
    - "Ningún archivo fuente que implemente la lógica de la aplicación supera las 300 líneas".
    - "main.go es solo arranque y cableado".
    - "App.jsx o punto de entrada de aplicación equivalente es solo composición/enrutamiento".
    - "Existen pruebas y los comandos se registran con códigos de salida".
    - "El control de calidad, AppSec y las puertas de lanzamiento no pueden pasar con evidencia únicamente narrativa".
    - "El estado final se puede completar sólo después de que pase la preparación para el lanzamiento".
  alcance:
    en_alcance:
      - "CRUD para entornos de aprendizaje, grupos de formación, instructores y horarios".
      - “Clasificación de profesores por tipo de contrato”.
      - “Registro de observaciones operativas vinculadas a horarios o instructores”.
      - "Interfaces de usuario básicas para coordinadores".
      - "API de backend con validación, manejo de errores y persistencia de datos".
      - "Containerización de todos los servicios utilizando Docker y Docker Compose".
      - "Cumplimiento de estándares arquitectónicos, de seguridad y de calidad según reglas no negociables".
    fuera_de_alcance:
      - "Algoritmos de programación complejos (por ejemplo, resolución automatizada de conflictos, optimización)".
      - "Paneles de informes o análisis avanzados".
      - "Autenticación de usuario más allá de la separación de roles básica para MVP".
      - "Notificaciones en tiempo real o funciones de chat".
      - "Internacionalización (i18n) para etiquetas de cara al usuario en varios idiomas (se permite el español, pero no un sistema i18n completo)".
      - "Capacidades sin conexión para frontend".
  partes interesadas:
    - "Coordinador Académico (Usuario Principal)"
    - "Instructor (usuario secundario)"
    - "Administrador (Usuario Operacional)"
    - "Propietario del producto"
    - "Equipo de desarrollo"
    - "Equipo de control de calidad"
    - "Equipo de seguridad"
    - "Equipo de Operaciones"
  puntuación de calidad: 0,95
```