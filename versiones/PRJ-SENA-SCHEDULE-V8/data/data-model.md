```yaml
modelo_datos:
  data_model_document: "Este documento describe el modelo de datos lógico para SENA Schedule Manager v8, basado en las pautas PRD y Clean Architecture proporcionadas. Define entidades, relaciones, restricciones propuestas, identificación de PII, reglas de retención y patrones de consulta comunes. El modelo se adhiere a las convenciones de nomenclatura especificadas: inglés, singular, lower_snake_case para todas las entidades y atributos".
  entidades:
    - nombre: instructor
      descripción: "Representa a un instructor dentro del centro de capacitación. Puede ser personal o contratista."
      atributos:
        - nombre: identificación
          tipo: Id. de objeto
          descripción: "Identificador único para el instructor."
          is_primary_key: verdadero
          is_required: verdadero
        - nombre: nombre
          tipo: cadena
          descripción: "Nombre completo del instructor."
          is_required: verdadero
          longitud_máxima: 100
        - nombre: correo electrónico
          tipo: cadena
          descripción: "Dirección de correo electrónico del instructor."
          is_required: verdadero
          is_unique: verdadero
          regex_pattern: "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$"
          longitud_máxima: 100
        - nombre: tipo
          tipo: cadena
          descripción: "Tipo de instructor: 'personal' o 'contratista'."
          is_required: verdadero
          valores_permitidos: ["personal", "contratista"]
        - nombre: área_experiencia
          tipo: matriz
          tipo_artículos: cadena
          descripción: "Lista de áreas de especialización del instructor."
          is_required: falso
          max_items: 10
          max_length_item: 50
    - nombre: entorno
      descripción: "Representa un entorno de aprendizaje donde se puede llevar a cabo la capacitación (por ejemplo, aula, laboratorio)".
      atributos:
        - nombre: identificación
          tipo: Id. de objeto
          descripción: "Identificador único para el entorno de aprendizaje."
          is_primary_key: verdadero
          is_required: verdadero
        - nombre: nombre
          tipo: cadena
          descripción: "Nombre o identificador del entorno (p. ej., 'Aula A', 'Laboratorio 2')."
          is_required: verdadero
          is_unique: verdadero
          longitud_max: 50
        - nombre: capacidad
          tipo: Número
          descripción: "Número máximo de individuos que el entorno puede albergar."
          is_required: verdadero
          valor_mínimo: 1
        - nombre: tipo
          tipo: cadena
          descripción: "Tipo de entorno (p. ej., 'aula', 'laboratorio', 'virtual')."
          is_required: verdadero
          longitud_max: 50
        - nombre: elemento_equipo
          tipo: matriz
          tipo_artículos: cadena
          descripción: "Lista de elementos de equipo clave disponibles en el medio ambiente".
          is_required: falso
          max_items: 20
          max_length_item: 50
    - nombre: grupo_entrenamiento
      descripción: "Representa un grupo de alumnos asociados con un programa de capacitación específico."
      atributos:
        - nombre: identificación
          tipo: Id. de objeto
          descripción: "Identificador único para el grupo de entrenamiento."
          is_primary_key: verdadero
          is_required: verdadero
        - nombre: nombre
          tipo: cadena
          descripción: "Nombre o código del grupo de formación."
          is_required: verdadero
          is_unique: verdadero
          longitud_máxima: 100
        - nombre: cuenta_estudiantes
          tipo: Número
          descripción: "Número de estudiantes en el grupo."
          is_required: verdadero
          valor_mínimo: 1
        - nombre: fecha_inicio
          tipo: Fecha
          descripción: "Fecha de inicio del programa del grupo de formación."
          is_required: verdadero
        - nombre: fecha_finalización
          tipo: Fecha
          descripción: "Fecha de finalización del programa del grupo de formación."
          is_required: verdadero
          regla_validación: "fecha_finalización >= fecha_inicial"
    - nombre: horario
      descripción: "Representa una sesión de capacitación programada, que vincula a un instructor, grupo y entorno durante un tiempo específico."
      atributos:
        - nombre: identificación
          tipo: Id. de objeto
          descripción: "Identificador único para la entrada del cronograma."
          is_primary_key: verdadero
          is_required: verdadero
        - nombre: instructor_id
          tipo: Id. de objeto
          descripción: "Referencia al instructor asignado a este horario."
          is_required: verdadero
          Foreign_key_to: instructor
        - nombre: entrenamiento_grupo_id
          tipo: Id. de objeto
          descripción: "Referencia al grupo de entrenamiento para este horario."
          is_required: verdadero
          clave_extranjera_para: grupo_entrenamiento
        - nombre: id_entorno
          tipo: Id. de objeto
          descripción: "Referencia al entorno de aprendizaje para este horario."
          is_required: verdadero
          Foreign_key_to: entorno
        - nombre: hora_inicio
          tipo: Fecha
          descripción: "Marca de tiempo de inicio de la sesión programada."
          is_required: verdadero
        - nombre: hora_final
          tipo: Fecha
          descripción: "Marca de tiempo de finalización de la sesión programada."
          is_required: verdadero
          regla_validación: "hora_final > hora_inicio"
        - nombre: estado
          tipo: cadena
          descripción: "Estado actual del programa (por ejemplo, 'programado', 'completado', 'cancelado')."
          is_required: verdadero
          valores_permitidos: ["programado", "completado", "cancelado"]
          default_value: "programado"
        - nombre: descripción
          tipo: cadena
          descripción: "Breve descripción del contenido de la sesión."
          is_required: falso
          longitud_máxima: 250
    - nombre: observación
      descripción: "Representa una observación o comentario operativo, vinculado a un instructor o un cronograma."
      atributos:
        - nombre: identificación
          tipo: Id. de objeto
          descripción: "Identificador único para la observación."
          is_primary_key: verdadero
          is_required: verdadero
        - nombre: descripción
          tipo: cadena
          descripción: "Descripción detallada de la observación."
          is_required: verdadero
          longitud_máxima: 500
        - nombre: fecha_observación
          tipo: Fecha
          descripción: "Fecha y hora en que se realizó la observación."
          is_required: verdadero
          valor_predeterminado: "ahora()"
        - nombre: observado_por
          tipo: cadena
          descripción: "Identificador del usuario que realizó la observación (por ejemplo, ID del coordinador)."
          is_required: verdadero
          longitud_máxima: 100
        - nombre: tipo_referencia
          tipo: cadena
          descripción: "Indica si la observación se refiere a un 'horario' o 'instructor'."
          is_required: verdadero
          valores_permitidos: ["horario", "instructor"]
        - nombre: id_referencia
          tipo: Id. de objeto
          descripción: "ID del programa o instructor al que se refiere esta observación".
          is_required: verdadero
          referencias_polimórficas:
            - entidad: horario
              campo: identificación
            - entidad: instructor
              campo: identificación
  relaciones:
    - nombre: instructor_to_schedule
      from_entity: instructor
      from_attribute: identificación
      a_entidad: programar
      to_attribute: instructor_id
      tipo: uno a muchos
      descripción: "Se puede asignar un instructor a múltiples horarios."
    - nombre: grupo_entrenamiento_al_programa
      de_entidad: grupo_entrenamiento
      from_attribute: identificación
      a_entidad: programar
      to_attribute: entrenamiento_grupo_id
      tipo: uno a muchos
      descripción: "Un grupo de entrenamiento se puede asociar con múltiples horarios."
    - nombre: entorno_a_programación
      from_entity: entorno
      from_attribute: identificación
      a_entidad: programar
      to_attribute: entorno_id
      tipo: uno a muchos
      descripción: "Un entorno puede albergar múltiples programaciones."
    - nombre: observación_a_entidad
      from_entity: observación
      from_attribute: referencia_id
      to_entity: null # Relación polimórfica, la entidad de destino depende del tipo de referencia
      to_attribute: nulo
      tipo: Polimórfico
      descripción: "Una observación se puede vincular a un horario o a un instructor."
  campos_pii:
    - entidad: instructor
      atributo: nombre
      motivo: "Identificador directo de una persona".
      Privacy_level: "Confidencial"
      handle_guidance: "Requiere control de acceso. Enmascarar en registros. Cifrar en reposo."
    - entidad: instructor
      atributo: correo electrónico
      motivo: "Información de contacto directo, identificador único".
      Privacy_level: "Confidencial"
      handle_guidance: "Requiere control de acceso. Enmascarar en los registros. Cifrar en reposo. Se utiliza para la comunicación."
    - entidad: observación
      atributo: observado_por
      motivo: "Identificador de la persona que realiza la observación".
      nivel_privacidad: "Interno"
      handle_guidance: "Requiere control de acceso interno. Debe ser el ID de usuario del sistema, no el nombre completo, si es posible."
  reglas_retencion:
    - entidad: horario
      regla: "Conservar durante 5 años después del tiempo de finalización, luego archivar o eliminar temporalmente. Registro de auditoría operativa".
      justificación: "Registro histórico de progreso académico y utilización de recursos".
    - entidad: observación
      regla: "Conservar durante 5 años después de la fecha_de observación. Asociado con horarios/instructores".
      justificación: "Documentación de revisión de cumplimiento y desempeño".
    - entidad: instructor
      regla: "Conservar durante 7 años después de la última programación activa, luego anonimizar los datos personales".
      justificación: "Potencial legal y de reactivación. Anonimizar los campos sensibles para cumplir con la minimización de datos".
    - entidad: grupo_formación
      regla: "Conservar durante 7 años después de la fecha de finalización, luego archivar o eliminar temporalmente".
      justificación: "Contexto histórico de los programas educativos".
    - entidad: medio ambiente
      regla: "Conservar indefinidamente o hasta el desmantelamiento de la instalación".
      justificación: "Datos de referencia estáticos, cambian con poca frecuencia".
  patrones_de consulta:
    - nombre: "Obtener horarios de instructores"
      descripción: "Recuperar todos los horarios de un instructor específico dentro de un rango de fechas."
      entidades_involucradas: ["instructor", "horario"]
      campos_filtrados: ["schedule.instructor_id", "schedule.start_time", "schedule.end_time"]
      índices_propuestos:
        - recogida: horario
          campos: ["instructor_id", "start_time", "end_time"]
          tipo: compuesto
    - nombre: "Buscar entornos disponibles"
      descripción: "Buscar entornos disponibles para un intervalo de tiempo y capacidad determinados".
      entidades_involucradas: ["entorno", "horario"]
      campos_filtrados: ["schedule.start_time", "schedule.end_time", "environment.capacity"]
      índices_propuestos:
        - recogida: horario
          campos: ["hora_inicio", "hora_finalización"]
          tipo: compuesto
        - colección: medio ambiente
          campos: ["capacidad"]
          tipo: soltero
    - nombre: "Obtener descripción general del horario del grupo"
      descripción: "Recuperar todos los horarios de un grupo de entrenamiento específico."
      entidades_involucradas: ["grupo_entrenamiento", "horario"]
      campos_filtrados: ["programa.training_group_id"]
      índices_propuestos:
        - recogida: horario
          campos: ["training_group_id"]
          tipo: soltero
    - nombre: "Lista de observaciones para el instructor/horario"
      descripción: "Recupera todas las observaciones relacionadas con un instructor o horario específico."
      entidades_involucradas: ["observación"]
      campos_filtrados: ["observación.tipo_referencia", "observación.id_referencia"]
      índices_propuestos:
        - colección: observación
          campos: ["tipo_referencia", "id_referencia"]
          tipo: compuesto
  puntuación de calidad: 0,95
```