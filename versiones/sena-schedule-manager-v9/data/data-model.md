```yaml
modelo_datos:
  documento_modelo_datos: |
    # Administrador de Horarios SENA - Modelo de Datos
    Este documento define el modelo de datos lógicos para el sistema MVP basado en MongoDB.
    Las colecciones se nombran en forma singular en inglés según sea necesario.
    El modelo admite salas, grupos de capacitación, instructores, bloques de programación y entidades de observación.
    La detección de conflictos se maneja a través de índices en ScheduleBlock para intervalos de tiempo superpuestos por sala, grupo o instructor.
    Los campos de PII están identificados para instructores y autores de observaciones.
    Las reglas de retención conservan los datos durante 5 años después de la desactivación (o cancelación) para fines legales o de auditoría.
  entidades:
    - nombre: habitación
      descripción: Representa una sala física o virtual disponible para programación.
      atributos:
        - nombre: identificación
          tipo: Id. de objeto
          requerido: verdadero
          descripción: Identificador único generado por MongoDB.
        - nombre: nombre
          tipo: cadena
          requerido: verdadero
          descripción: Nombre de la habitación legible por humanos (p. ej., "Aula 101").
        - nombre: capacidad
          tipo: entero
          requerido: falso
          descripción: Número máximo de personas que puede albergar la habitación.
        - nombre: ubicación
          tipo: cadena
          requerido: falso
          descripción: Ubicación física o edificio.
        - nombre: elemento_equipo
          tipo: matriz
          tipo_artículos: cadena
          requerido: falso
          descripción: Listado de equipos disponibles en la sala (p. ej., "proyector", "pizarra").
        - nombre: is_active
          tipo: booleano
          requerido: verdadero
          descripción: Indica si la sala está disponible para programación. Las habitaciones para discapacitados no son asignables.
        - nombre: creado_at
          tipo: fecha y hora
          requerido: verdadero
          descripción: Marca de tiempo de creación.
        - nombre: actualizado_at
          tipo: fecha y hora
          requerido: verdadero
          descripción: Marca de tiempo de la última actualización.
    - nombre: grupo_entrenamiento
      descripción: Representa un grupo de estudiantes que reciben capacitación juntos.
      atributos:
        - nombre: identificación
          tipo: Id. de objeto
          requerido: verdadero
          descripción: Identificador único.
        - nombre: nombre
          tipo: cadena
          requerido: verdadero
          descripción: nombre para mostrar del grupo.
        - nombre: código
          tipo: cadena
          requerido: verdadero
          descripción: Código institucional del grupo.
        - nombre: cuenta_estudiantes
          tipo: entero
          requerido: falso
          descripción: Número de estudiantes matriculados. (nombre singular según reglas de nomenclatura)
        - nombre: is_active
          tipo: booleano
          requerido: verdadero
          descripción: Indica si el grupo está activo.
        - nombre: creado_at
          tipo: fecha y hora
          requerido: verdadero
          descripción: Marca de tiempo de creación.
        - nombre: actualizado_at
          tipo: fecha y hora
          requerido: verdadero
          descripción: Marca de tiempo de la última actualización.
    - nombre: instructor
      descripción: Representa a un instructor permanente o contratista.
      atributos:
        - nombre: identificación
          tipo: Id. de objeto
          requerido: verdadero
          descripción: Identificador único.
        - nombre: nombre
          tipo: cadena
          requerido: verdadero
          descripción: Nombre del instructor.
        - nombre: apellido
          tipo: cadena
          requerido: verdadero
          descripción: Apellido del instructor.
        - nombre: correo electrónico
          tipo: cadena
          requerido: verdadero
          descripción: Dirección de correo electrónico (PII).
          pii: cierto
        - nombre: teléfono
          tipo: cadena
          requerido: falso
          descripción: Número de teléfono (PII).
          pii: cierto
        - nombre: documento_id
          tipo: cadena
          requerido: verdadero
          descripción: Número de identificación nacional (PII).
          pii: cierto
        - nombre: tipo_contratación
          tipo: cadena
          requerido: verdadero
          descripción: "Tipo de contrato: 'permanente' o 'contratista'."
        - nombre: área_experiencia
          tipo: matriz
          tipo_artículos: cadena
          requerido: falso
          descripción: Áreas de especialización (p. ej., "programación", "redes").
        - nombre: is_active
          tipo: booleano
          requerido: verdadero
          descripción: Indica si se puede asignar el instructor.
        - nombre: creado_at
          tipo: fecha y hora
          requerido: verdadero
          descripción: Marca de tiempo de creación.
        - nombre: actualizado_at
          tipo: fecha y hora
          requerido: verdadero
          descripción: Marca de tiempo de la última actualización.
    - nombre: horario_bloque
      Descripción: Representa un intervalo de tiempo programado para una sala, grupo e instructor.
      atributos:
        - nombre: identificación
          tipo: Id. de objeto
          requerido: verdadero
          descripción: Identificador único.
        - nombre: room_id
          tipo: Id. de objeto
          requerido: verdadero
          descripción: Referencia a la habitación asignada.
        - nombre: entrenamiento_grupo_id
          tipo: Id. de objeto
          requerido: verdadero
          descripción: Referencia al grupo de formación asignado.
        - nombre: instructor_id
          tipo: Id. de objeto
          requerido: verdadero
          descripción: Referencia al instructor asignado.
        - nombre: hora_inicio
          tipo: fecha y hora
          requerido: verdadero
          Descripción: Fecha y hora de inicio del bloque.
        - nombre: hora_final
          tipo: fecha y hora
          requerido: verdadero
          Descripción: Fecha y hora de finalización del bloque. Debe ser posterior a start_time.
        - nombre: estado
          tipo: cadena
          requerido: verdadero
          descripción: "Estado del bloqueo: 'programado', 'cancelado'. MVP puede extenderse."
        - nombre: está_cancelado
          tipo: booleano
          requerido: verdadero
          descripción: Derivado del estado; permite un filtrado rápido.
        - nombre: creado_at
          tipo: fecha y hora
          requerido: verdadero
          descripción: Marca de tiempo de creación.
        - nombre: actualizado_at
          tipo: fecha y hora
          requerido: verdadero
          descripción: Marca de tiempo de la última actualización.
    - nombre: observación
      descripción: Observación adjunta a un bloque de programación.
      atributos:
        - nombre: identificación
          tipo: Id. de objeto
          requerido: verdadero
          descripción: Identificador único.
        - nombre: Schedule_block_id
          tipo: Id. de objeto
          requerido: verdadero
          descripción: Referencia al bloque de programación asociado.
        - nombre: autor
          tipo: cadena
          requerido: verdadero
          descripción: Nombre de la persona que escribió la observación (PII).
          pii: cierto
        - nombre: texto
          tipo: cadena
          requerido: verdadero
          descripción: Contenido en texto libre de la observación.
        - nombre: gravedad
          tipo: cadena
          requerido: verdadero
          descripción: "Nivel de gravedad: 'información', 'advertencia', 'crítico'."
        - nombre: creado_at
          tipo: fecha y hora
          requerido: verdadero
          descripción: Marca de tiempo de creación.
  relaciones:
    - nombre: agenda_bloque_habitación
      Descripción: Hay un bloque de horario asignado exactamente a una habitación.
      entidad_fuente: bloque_programación
      entidad_objetivo: habitación
      cardinalidad: muchos a uno
      clave_extranjera: id_habitación
    - nombre: Schedule_block_training_group
      Descripción: Se asigna un bloque de programación exactamente a un grupo de entrenamiento.
      entidad_fuente: bloque_programación
      entidad_objetivo: grupo_entrenamiento
      cardinalidad: muchos a uno
      clave_extranjera: id_grupo_entrenamiento
    - nombre: horario_bloque_instructor
      Descripción: Se asigna un bloque de programación exactamente a un instructor.
      entidad_fuente: bloque_programación
      entidad_objetivo: instructor
      cardinalidad: muchos a uno
      clave_extranjera: id_instructor
    - nombre: observe_schedule_block
      descripción: Una observación pertenece exactamente a un bloque de programación.
      entidad_fuente: observación
      entidad_objetivo: bloque_programación
      cardinalidad: muchos a uno
      clave_extranjera: Schedule_block_id
  campos_pii:
    - entidad: instructor
      campos:
        - correo electrónico
        - teléfono
        - id_documento
      justificación: "El correo electrónico, el teléfono y el documento de identificación se consideran información de identificación personal según la ley colombiana de protección de datos (Ley 1581)".
    - entidad: observación
      campos:
        - autor
      justificación: "El campo de autor almacena el nombre de una persona física y, por lo tanto, es PII".
  reglas_retencion:
    - entidad: habitación
      regla: "Las salas activas se conservan indefinidamente. Las salas deshabilitadas se conservan durante 5 años después de la fecha de desactivación y luego se eliminan temporalmente (archivadas)".
      duración: "5 años después de la desactivación"
    - entidad: grupo_formación
      regla: "Los grupos activos se conservan indefinidamente. Los grupos deshabilitados se conservan durante 5 años después de la desactivación".
      duración: "5 años después de la desactivación"
    - entidad: instructor
      regla: "Los instructores activos se retienen indefinidamente. Los instructores discapacitados se retienen durante 5 años después de la desactivación, luego anonimizan los campos de PII".
      duración: "5 años después de la desactivación, luego anonimizar"
    - entidad: horario_bloque
      regla: "Los bloques programados se retienen durante 5 años después de la hora_final. Los bloques cancelados se retienen durante 2 años y luego se eliminan".
      duración: "5 años (programado), 2 años (cancelado)"
    - entidad: observación
      regla: "Las observaciones se conservan durante la misma duración que el bloque de programación principal".
      duración: "Heredado de Schedule_block"
  patrones_de consulta:
    - nombre: detección_conflicto
      descripción: "Buscar bloques de programación superpuestos para una sala, grupo o instructor determinado dentro de un rango de tiempo".
      colecciones:
        - horario_bloque
      índices_usados:
        - Schedule_block_room_time
        - Schedule_block_training_group_time
        - horario_bloque_instructor_time
      example_query: "{ room_id: <id>, start_time: { $lt: <end> }, end_time: { $gt: <start> }, is_cancelled: false }"
    - nombre: lista_entidades_activas
      descripción: "Enumera todas las salas, grupos o instructores activos."
      colecciones:
        - habitación
        - grupo_entrenamiento
        - instructor
      filtro: "{ is_active: verdadero }"
    - nombre: Schedule_blocks_for_room
      descripción: "Recuperar todos los bloques de programación para una sala específica, ordenados por tiempo."
      colecciones:
        - horario_bloque
      índice: "schedule_block_room_time"
    - nombre: observaciones_para_bloque
      descripción: "Obtener todas las observaciones para un bloque de programación determinado."
      colecciones:
        - observación
      índice: "observation_schedule_block_id"
  puntuación_calidad:
    puntuación: 95
    razones:
      - "Todos los nombres de entidades y atributos están en inglés, singular, lower_snake_case y cumplen con las reglas de nomenclatura".
      - "Los campos PII están marcados y justificados explícitamente".
      - “Se definen políticas de retención para cada entidad con duraciones”.
      - "Se identifican índices para la detección de conflictos y consultas comunes".
      - "Las relaciones están claramente definidas con claves foráneas".
      - "Deducción menor: la entidad de observación utiliza 'autor' como PII; podría anonimizarse aún más. Aún no se ha incluido ninguna implementación explícita de eliminación temporal".
```