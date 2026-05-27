```yaml
requisitos:
  prd: |
    # Documento de requisitos del producto
    ## Proyecto
    Responsable de Horarios del SENA

    ## Objetivo del producto
    Construir un MVP en forma de producción para gestionar los cronogramas académicos del SENA. MVP significa alcance funcional reducido, no calidad de ingeniería reducida.
    El producto debe admitir salas de programación, grupos de capacitación, instructores, bloques de programación y observaciones. La interfaz de usuario y la documentación del usuario deben estar en español. Todo el código técnico, nombres de carpetas, nombres de objetos de bases de datos, nombres de entidades, atributos, diagramas, contratos API, pruebas, comentarios y documentación para desarrolladores deben estar en inglés.

    ## Pila obligatoria
    - Servidor: Ir
    - Interfaz: Reaccionar
    - Base de datos: MongoDB
    - Tiempo de ejecución: Docker y Docker Compose

    ## Límites de los componentes
    - base de datos/ – inicio de MongoDB, semilla, índices, validación, Dockerfile.
    - backend/ – Ir solo a API.
    - frontend/ – Reaccionar UI solamente.
    - Raíz: docker-compose.yml, .env.example, .gitignore, documentos entre componentes.
    Cada componente tiene su propio contenedor.

    ## Alcance funcional
    - Salas: CRUD + desactivar, detectar conflictos por sala y hora.
    - Grupos de Entrenamiento: CRUD + desactivar, detectar conflictos por grupo y tiempo.
    - Instructores: CRUD + inhabilitación (permanente/contratista), detectar conflictos por instructor y tiempo.
    - Horarios: CRUD + cancelar, validar fin>inicio, prevenir conflictos.
    - Observaciones: Agregar a bloques de programación (texto, autor, fecha, gravedad: información/advertencia/crítica).

    ## Requisitos de arquitectura
    - Capas de backend: cmd/api, internal/{config,dominio,aplicación,http,repositorio,infraestructura}. Reglas estrictas de dependencia.
    - Funciones de frontend: src/{app,features,components,services,hooks,types}. Llamadas API a componentes externos.
    - Base de datos: nombres de colecciones singulares en inglés (sala, grupo de entrenamiento, etc.), scripts de inicio propios, semillas, índices.

    ## Requisitos de calidad
    - Pruebas unitarias backend para reglas de dominio y detección de conflictos.
    - Pruebas de frontend para cliente API e interacciones clave de UI.
    - Comandos ejecutables de compilación/prueba con cwd, ejecución, propósito y código de salida esperado.
    - Informes de control de calidad que citan comandos/archivos; AppSec informa que cita controles específicos.
    - Preparación para el lanzamiento: Docker Compose, comprobaciones de estado, .env.example, implementación/reversión, inicio reproducible.

    ## Criterios de aceptación
    - Docker Compose Up: Build inicia todos los servicios.
    - La verificación de estado del backend verifica la accesibilidad de la dependencia.
    - Prevención completa de conflictos CRUD+ vía API.
    - Pruebas/comandos ejecutables desde caja limpia.
    - Sin archivos monolóticos; sin nombres técnicos españoles.
    - El estado final solo se completó con evidencia verificable de control de calidad/AppSec/liberación.

  requisitos_funcionales:
    - "Gestión de salas: crear, listar, actualizar, desactivar salas con código, nombre, capacidad, ubicación, estado activo".
    - "Gestión de grupos de entrenamiento: Crear, listar, actualizar, deshabilitar grupos con código, nombre del programa, fechas de inicio/finalización, estado activo".
    - "Gestión de instructores: Crear, listar, actualizar, desactivar instructores con nombre completo, correo electrónico, tipo de contrato (permanente/contratista), especialización, estado activo".
    - "Gestión de bloques de programación: crear, enumerar, actualizar, cancelar bloques con fecha, hora de inicio/finalización, sala, grupo, instructor, actividad de aprendizaje. Validar fin > inicio. Evite conflictos entre sala, grupo e instructor".
    - "Observaciones: Agregar observaciones a los bloques de programación con texto, autor, fecha de creación, gravedad (información, advertencia, crítica)."
    - "Detección de conflictos: no aplique doble reserva de sala, grupo de capacitación o instructor dentro de rangos de tiempo superpuestos".

  requisitos_no_funcionales:
    - "El backend debe utilizar una arquitectura Go en capas (cmd, internal/{config,domain,application,http,repository,infrastructure})."
    - "Los controladores HTTP no deben acceder a MongoDB directamente; utilice interfaces de repositorio".
    - "El código de dominio no debe importar paquetes HTTP, DB o framework".
    - "El servidor debe incluir tiempos de espera de lectura/escritura/inactividad/apagado".
    - "CORS no debe ser un comodín en el modo de producción".
    - "500 respuestas no deben exponer errores internos sin procesar".
    - "Archivos fuente <300 líneas".
    - "El frontend debe utilizar una arquitectura React orientada a funciones (aplicaciones, funciones, componentes, servicios, enlaces, tipos)".
    - "Las llamadas API se encuentran fuera de los componentes de React; el cliente API tiene tiempo de espera y manejo de errores estructurado".
    - "Copia de UI en español, nombres en clave en inglés".
    - "Las colecciones de MongoDB utilizan nombres singulares en inglés (sala, grupo de formación, instructor, bloque de programación, observación)".
    - "Scripts de inicio/semilla de base de datos almacenados en la base de datos/carpeta únicamente, no en el backend/".
    - "Incluir pruebas unitarias de backend para reglas de dominio y detección de conflictos".
    - "Incluir pruebas de interfaz para el cliente API y las interacciones clave de la interfaz de usuario".
    - "Comandos de prueba ejecutables desde una caja limpia con cwd, ejecución, propósito y código de salida esperado documentados".
    - "Los informes de control de calidad deben citar comandos, códigos de salida y archivos reales".
    - "Los informes de AppSec deben citar archivos, líneas/controles concretos".
    - "La preparación para el lanzamiento debe verificar Docker Compose, comprobaciones de estado, .env.example, implementación, reversión, inicio reproducible".

  historias_de_usuario:
    - "Como administrador de horarios, quiero crear y administrar salas para poder asignar espacios físicos a los bloques de horarios".
    - "Como administrador de programación, quiero crear y administrar grupos de capacitación para poder asignar cohortes a bloques de programación".
    - "Como administrador de horarios, quiero crear y administrar instructores para poder asignar personal calificado a los bloques de horarios".
    - "Como administrador de horarios, quiero crear bloques de horarios con asignaciones de sala/grupo/instructor y que el sistema evite conflictos".
    - "Como administrador de programación, quiero agregar observaciones a los bloques de programación (información, advertencia, críticos) para comunicar problemas".
    - "Como administrador de programación, quiero ver un punto final de verificación de estado que confirme que el backend está conectado a la base de datos".

  criterios_de_aceptación:
    - "Docker Compose Up --build inicia contenedores de base de datos, backend y frontend".
    - "La comprobación de estado del backend devuelve 200 sólo cuando se puede acceder a MongoDB".
    - "El frontend puede enumerar y crear salas, instructores, grupos de capacitación, bloques de programación y observaciones a través de la API de backend".
    - "Las reglas de conflicto impiden la doble reserva de una sala, instructor o grupo de capacitación para rangos de tiempo superpuestos".
    - "Todas las pruebas (pruebas unitarias de backend, pruebas de frontend) pasan cuando se ejecutan desde una caja limpia con los comandos proporcionados".
    - "Ningún componente se entrega como un archivo monolítico (cada proyecto tiene subcarpetas adecuadas)".
    - "Ningún código técnico u objeto de esquema de base de datos utiliza nombres en español (por ejemplo, los nombres de las colecciones están en singular en inglés)".
    - "El estado final no se marca como 'completado' a menos que el control de calidad, AppSec y las puertas de lanzamiento pasen con evidencia verificable (salidas de comandos, citas de archivos)".

  alcance:
    en_alcance:
      - "Sala CRUD y detección de conflictos"
      - “Grupo de formación CRUD y detección de conflictos”
      - "Instructor CRUD (con tipos permanente/contratado) y detección de conflictos"
      - "Programación de bloques CRUD, validación (fin > inicio) y prevención de conflictos"
      - "Creación de observaciones en cronogramas con niveles de severidad"
      - "Backend Go en capas con MongoDB"
      - "Frontal de React orientado a funciones con interfaz de usuario en español"
      - "Orquestación de Docker Compose con controles de salud"
      - "Pruebas unitarias para reglas de dominio y detección de conflictos"
      - "Pruebas de frontend para cliente API e interacciones clave"
      - "Datos semilla y scripts de inicialización en inglés"
    fuera_de_alcance:
      - "Autenticación y autorización de usuario (sin inicio de sesión/roles)"
      - "Notificaciones en tiempo real o alertas por correo electrónico"
      - "Paneles de informes o análisis"
      - "Soporte multilingüe (solo UI en español)"
      - "Optimización del diseño móvil o responsivo"
      - "Integración con calendarios externos (por ejemplo, Google Calendar)"
      - "Algoritmos de optimización u programación automática"
      - "Registro de auditoría más allá del historial de observación"

  partes interesadas:
    - “Coordinadores académicos y gestores de agenda del SENA”
    - "Instructores (usuarios finales que ven y están asignados a horarios)"
    - "Equipo de operaciones TI del SENA (implementar y mantener el sistema)"
    - "Equipo de aseguramiento de calidad (verificar requisitos y pruebas)"
    - "Equipo de seguridad (revisar controles de AppSec)"
    - "Equipo de desarrollo (implementar y entregar el MVP)"

  puntuación de calidad: 92
```