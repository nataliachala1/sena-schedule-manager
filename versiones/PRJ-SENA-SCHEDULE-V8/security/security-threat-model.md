```yaml
modelo_amenaza_seguridad:
  casos_de_abuso:
  - descripción: un usuario no autorizado intenta crear, actualizar o eliminar horarios,
      instructores, entornos o grupos de formación.
    mitigation_strategy: "Implementar comprobaciones de autenticación y autorización de API Gateway. \
      La capa de "transporte" del backend debe aplicar el control de acceso basado en roles (RBAC) en \
      todos los puntos finales basados en los `user_roles` especificados en el PRD (Coordinador Académico, \
      Administrador, Instructor). Por ejemplo, solo los coordinadores/administradores pueden crear/actualizar/eliminar \
      horarios o instructores."
    nombre: Manipulación de datos no autorizada
  - descripción: un instructor intenta ver horarios u observaciones no asignadas
      a ellos, o modificar sus propios horarios.
    mitigation_strategy: "Implementar control de acceso granular basado en atributos (ABAC) \
      o autorización basada en recursos. La capa `aplicación/caso de uso` debe verificar \
      que el `instructor_id` autenticado coincida con el `instructor_id` en el \ solicitado
      programación u observación antes de permitir el acceso de lectura/actualización".
    nombre: Escalada de privilegios/violación de acceso a datos (instructor)
  - descripción: usuario malintencionado inyecta script o comandos SQL/NoSQL en campos de entrada
      (por ejemplo, `nombre`, `descripción`, `correo electrónico`, `área_experta`) para comprometer los datos
      o aplicación.
    mitigation_strategy: "Implemente una sólida validación y desinfección de entradas en el \
      capa `infraestructura/transporte` (para DTO) y dentro de `dominio/entidad` para \
      reglas de negocio. Utilice consultas parametrizadas/funciones ORM del controlador Go MongoDB. \
      Escapa de todos los resultados renderizados en la interfaz. Aplicar la validación del esquema JSON en \
      Nivel MongoDB."
    nombre: Ataque de inyección
  - descripción: un atacante crea solicitudes para explotar vulnerabilidades en el entorno
      o creación de horarios, lo que lleva al agotamiento de los recursos o a la denegación de servicio (DoS).
    mitigation_strategy: "Implementar limitación de velocidad en los puntos finales de API, especialmente creación/actualización\
      puntos finales. Aplicar una validación de datos estricta para entradas numéricas (por ejemplo, "capacidad", \
      `student_count`) y rangos de fechas (por ejemplo, `start_time`, `end_time`) para evitar \
      asignación excesiva de recursos o bucles infinitos en la lógica de programación. Implementar \
      Tiempos de espera de HTTP y tiempos de espera de consultas de bases de datos."
    nombre: Agotamiento de recursos / DoS
  - descripción: acceso a los campos de PII (`instructor.name`, `instructor.email`, `observation.observed_by`)
      por usuarios no autorizados o a través de canales inseguros.
    mitigation_strategy: "Aplicar estrictos controles de acceso a los campos de PII a través de RBAC/ABAC. \
      Cifre la PII en reposo en MongoDB. Enmascare la PII en todos los registros. Utilice comunicación segura \
      (HTTPS). Asegúrese de que la interfaz no almacene en caché la PII en un almacenamiento inseguro (por ejemplo, `localStorage`)".
    nombre: Exposición PII
  superficies_ataque:
  - descripción: aplicación frontend que se ejecuta en los navegadores de los usuarios e interactúa con el
      API de back-end.
    nombre: Interfaz (React UI)
  - descripción: Puntos finales de la API backend (`infraestructura/transporte`) expuestos al
      frontend y potencialmente otros servicios internos. Esta es la entrada principal.
      punto para toda la lógica empresarial.
    nombre: Puerta de enlace API backend/capa de transporte
  - descripción: base de datos MongoDB, incluidas las colecciones y el sistema de archivos subyacente.
      Accesible solo por el backend.
    nombre: Base de datos (MongoDB)
  - descripción: demonio Docker y orquestación de contenedores (Docker Compose).
    nombre: Tiempo de ejecución de contenedor/orquestación
  puntuación de calidad: 95
  controles_requeridos:
  - control: asegúrese de que todos los datos de entrada estén validados y desinfectados en el límite de la API
      y dentro de la capa de dominio.
    propietario: Equipo de backend
    verificación: "Implementar validación DTO en `infraestructura/transporte`. Dominio \
      las entidades (`dominio/entidad`) deben tener métodos de validación. Esquema JSON de MongoDB \
      Se debe aplicar la validación (`base de datos/validación de esquema`). Unidad e integración \
      pruebas para escenarios de validación de entradas".
  - control: implementar autenticación para todos los puntos finales de API.
    propietario: Equipo de backend
    verificación: "Todos los manejadores de `infraestructura/transporte` deben requerir una autenticación \
      ficha. Pruebas API automatizadas para acceso no autorizado (`401 No autorizado`) en \
      todos los puntos finales."
  - control: implementar control de acceso basado en roles (RBAC) para todas las operaciones confidenciales.
    propietario: Equipo de backend
    verificación: "Lógica de autorización en `aplicación/caso de uso` e `infraestructura/transporte` \
      debe verificar los roles de usuario (coordinador académico, administrador, instructor) con \
      acción. Pruebas API automatizadas para acceso prohibido (`403 Forbidden`) basadas en \
      papel."
  - control: cifrar datos PII en reposo en MongoDB.
    propietario: Equipo de base de datos / Equipo de backend
    verificación: "Cifrado a nivel de campo (FLE) de MongoDB para `instructor.name` y \
      `instructor.correo electrónico`. Las claves de cifrado deben administrarse de forma segura (KMS). Auditable \
      prueba de cifrado en reposo."
  - control: enmascarar PII en todos los registros de la aplicación.
    propietario: Equipo de backend
    verificación: "El middleware de registro debe detectar y enmascarar `instructor.name` y \
      `instructor.email` antes de iniciar sesión. Revisión manual de muestras de registros y automatizada \
      pruebas para el enmascaramiento de registros."
  - control: utilice HTTPS para todas las comunicaciones entre el frontend y el backend.
    propietario: Equipo de infraestructura
    verificación: "La configuración de implementación debe aplicar HTTPS. Validez del certificado \
      cheque. Pruebas de penetración para confirmar que no hay respaldo HTTP".
  - control: implementar encabezados HTTP seguros (por ejemplo, HSTS, CSP, X-Frame-Options) en
      el backend.
    propietario: Equipo de backend
    verificación: "Los encabezados de respuesta de `infraestructura/transporte` deben incluir \
      encabezados de seguridad. Informes del escáner de seguridad automatizado (por ejemplo, OWASP ZAP).
  - control: hacer cumplir políticas CORS estrictas.
    propietario: Equipo de backend
    verificación: "CORS debe restringirse a orígenes de interfaz conocidos en `infraestructura/transporte` \
      configuración. Pruebas manuales de solicitudes de origen cruzado de dominios no autorizados."
  - control: implementar limitación de velocidad en los puntos finales de API para evitar abusos.
    propietario: Equipo de backend
    verificación: "Middleware en `infraestructura/transporte` para limitar las solicitudes por \
      IP/usuario. Pruebas de rendimiento para verificar el comportamiento de limitación de velocidad bajo carga".
  - control: asegúrese de que las conexiones de la base de datos utilicen privilegios mínimos y una autenticación sólida.
    propietario: Equipo de base de datos / Equipo de backend
    verificación: "El usuario de MongoDB (`init-mongo.js`) debe tener solo los permisos necesarios. \
      Contraseñas/mecanismos seguros para el acceso a la base de datos. Prueba auditable de permisos. Conexión \
      La cuerda no debe quedar expuesta."
  - control: el backend debe ejecutarse en un contenedor que no sea raíz.
    propietario: Equipo DevOps
    verificación: "Dockerfile para backend debe especificar un usuario no root. Contenedor de tiempo de ejecución \
      inspecciones (`docker inspect`)".
  - control: el frontend debe evitar el almacenamiento inseguro del lado del cliente para datos confidenciales.
    propietario: Equipo Frontend
    verificación: "No se permite el uso de `localStorage` o `sessionStorage` para PII o tokens. \
      Inspección de herramientas de desarrollo del navegador. Escaneos de vulnerabilidades automatizados para el lado del cliente \
      almacenamiento."
  - control: implementar tiempos de espera para todas las llamadas externas (solicitudes HTTP, consultas de base de datos).
    propietario: Equipo de backend
    verificación: "El cliente `http.Client` de Go y el controlador MongoDB deben tener configurado \
      tiempos de espera. Revisión de código y pruebas unitarias para el manejo del tiempo de espera."
  - control: asegúrese de que los mensajes de error expuestos a los clientes sean genéricos y no se filtren
      información confidencial o seguimientos de pila.
    propietario: Equipo de backend
    verificación: "El manejo de errores en `infraestructura/transporte` debe devolver genérico \
      mensajes. Los registros internos pueden contener detalles completos. Pruebas de penetración para detectar errores \
      fuga de mensajes."
  - control: restringe el dominio/la lógica empresarial central para que no conozca la persistencia o
      Detalles del transporte.
    propietario: Equipo de backend
    verificación: "Revisión de código: los paquetes `dominio` y `aplicación` no deben importarse \
      `infraestructura/persistencia` o `infraestructura/transporte`. Análisis estático \
      herramientas (por ejemplo, `go vet`) y linters personalizados para hacer cumplir las reglas de inversión de dependencia".
  - control: Implementar registro y monitoreo de eventos y anomalías de seguridad.
    propietario: Equipo DevOps / Equipo Backend
    verificación: "Registro centralizado (por ejemplo, pila ELK). Alertas de inicio de sesión fallido\
      intentos, fallas de autorización, patrones de tráfico inusuales. Revisión periódica \
      de registros de seguridad."
  amenazas_zancadas:
  - descripción: un atacante podría hacerse pasar por un coordinador académico para realizar
      acciones no autorizadas en horarios, instructores o entornos.
    nombre: Falsificación de identidad (S)
    mitigation_guidance: "Autenticación sólida y verificación de identidad para los usuarios. \
      Implemente JWT para autenticación sin estado. Verificar firmas JWT y vencimiento \
      en cada solicitud. El backend debe validar `instructor_id` del token contra \
      recurso al que se accede/modifica para evitar la suplantación".
  - descripción: un atacante podría alterar los horarios programados, las tareas del instructor,
      u descripciones de observaciones, lo que lleva a datos incorrectos o maliciosos.
    nombre: Manipulación de datos (T)
    mitigation_guidance: "Validación y desinfección de entradas. Autorización sólida\
      controles (RBAC/ABAC) para garantizar que solo los usuarios autorizados puedan modificar los datos. Implementar \
      comprobaciones de integridad de datos, por ejemplo, aplicar la validación del esquema JSON de MongoDB. Registrar todo \
      modificaciones de datos críticos para la auditabilidad."
  - descripción: usuarios no autorizados podrían obtener acceso a PII (nombres de instructores, correos electrónicos)
      u observaciones operativas sensibles.
    nombre: Divulgación de Información (I)
    mitigation_guidance: "Cifrado de datos en reposo (MongoDB FLE) y en tránsito (HTTPS).\
      Estricto control de acceso (RBAC/ABAC). Enmascarar PII en registros. Evite devolver innecesarias \
      datos en las respuestas API. El frontend no debe almacenar PII en el lado del cliente inseguro \
      almacenamiento."
  - descripción: un atacante podría modificar el código de la aplicación o los archivos de configuración,
      o inyectar código malicioso a través de la entrada, lo que lleva a un comportamiento no autorizado o
      compromiso del sistema.
    nombre: Repudio (R)
    mitigation_guidance: "Registro completo de todas las acciones críticas con el usuario \
      ID, marca de tiempo y detalles de la acción. Canalización segura de construcción e implementación. Código \
      firma. Control de versiones para todo el código y configuraciones. Seguridad regular \
      auditorías."
  - descripción: Un atacante podría hacer que el servicio no esté disponible al abrumarlo
      con solicitudes o explotando vulnerabilidades que bloquean la aplicación/base de datos.
    nombre: Denegación de Servicio (D)
    mitigation_guidance: "Limitación de velocidad en puntos finales de API. Validación de entrada para uso intensivo de recursos \
      operaciones. Implemente tiempos de espera para procesos de larga duración o consultas de bases de datos. \
      Asegúrese de que se establezcan límites de recursos del contenedor. Manejo sólido de errores para prevenir \
      accidentes. Comprobaciones de estado y reinicios automatizados de servicios."
  - descripción: un atacante podría elevar sus privilegios, por ejemplo, un instructor
      cuenta obtiene acceso de Coordinador Académico o elude la autorización normal
      cheques.
    nombre: Elevación de Privilegio (E)
    mitigation_guidance: "Control de acceso granular (RBAC/ABAC) estrictamente aplicado \
      en la capa `aplicación/caso de uso`. Principio de privilegio mínimo para todos los usuarios \
      y servicios. Auditorías de seguridad periódicas y pruebas de penetración para identificar \
      rutas de escalada de privilegios."
  Threat_model_document: "El sistema SENA Schedule Manager v8, construido con Clean \
    Arquitectura en Go (backend), React (frontend) y MongoDB (base de datos), requiere \
    un modelo robusto de seguridad. Las principales superficies de ataque incluyen \
    el frontend (interacciones del usuario), el backend API (puntos de entrada a la \
    lógica de negocio), la base de datos (persistencia de datos sensibles) y el \
    tiempo de ejecución de contenedores. Las amenazas STRIDE se evalúan para garantizar \
    la confidencialidad, integridad y disponibilidad. Los controles de seguridad \
    se centran en la autenticación, autorización granular (RBAC/ABAC), validación \
    de entradas, cifrado de PII en reposo y en tránsito, enmascaramiento de PII \
    en logs, CORS estricto, rate limiting, gestión de secretos, ejecución de contenedores \
    sin privilegios y logging/auditoría. Los casos de abuso clave incluyen manipulación \
    de datos no autorizados, escalada de privilegios, inyección y exposición de PII. \
    Se exige la implementación de tiempos de espera HTTP/DB y mensajes de error genéricos. \
    La separación de preocupaciones importantes a Clean Architecture ayuda a segmentar \
    los controles de seguridad."
```