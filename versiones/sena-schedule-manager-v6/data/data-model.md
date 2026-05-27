```yaml
modelo_datos:
  documento_modelo_datos: |
    # Modelo de datos lógicos

    El modelo de datos MVP utiliza cinco raíces agregadas: aula, grupo_estudiante,
    Instructor, horario y observación. El agregado del cronograma hace referencia a la
    raíces del catálogo por identificador y posee validación específica del cronograma, como
    día, hora de inicio y hora de finalización. La observación pertenece al horario y a las tiendas.
    texto de seguimiento. Los nombres lógicos son inglés, singular y ASCII. Sin fisico
    Se requieren metadatos de MongoDB en los objetos de dominio; colección física y
    el diseño del índice se delega a A13, mientras que los adaptadores de persistencia del backend se asignan
    documentos a entidades de dominio puro.

    La PII se limita al correo electrónico del instructor y al número de documento. El MVP no
    incluir autenticación o gestión de roles, por lo que la identidad del coordinador está fuera
    de alcance. La retención es conservadora: se mantienen registros del cronograma operativo
    mientras el periodo de formación esté activo, luego se archivará según los requisitos académicos.
    política de registros.
  entidades:
    - nombre: aula
      Propósito: "Entorno de aprendizaje físico disponible para la programación"
      atributos:
        - nombre: identificación
          tipo: cadena
          requerido: verdadero
        - nombre: código
          tipo: cadena
          requerido: verdadero
        - nombre: nombre
          tipo: cadena
          requerido: verdadero
        - nombre: ubicación
          tipo: cadena
          requerido: verdadero
        - nombre: capacidad
          tipo: entero
          requerido: verdadero
    - nombre: grupo_estudiante
      Propósito: "Grupo de entrenamiento asignado a horarios"
      atributos:
        - nombre: identificación
          tipo: cadena
          requerido: verdadero
        - nombre: código
          tipo: cadena
          requerido: verdadero
        - nombre: nombre_programa
          tipo: cadena
          requerido: verdadero
        - nombre: cuenta_alumno
          tipo: entero
          requerido: verdadero
    - nombre: instructor
      Propósito: "Instructor asignado a sesiones de capacitación"
      atributos:
        - nombre: identificación
          tipo: cadena
          requerido: verdadero
        - nombre: número_documento
          tipo: cadena
          requerido: verdadero
        - nombre: nombre_completo
          tipo: cadena
          requerido: verdadero
        - nombre: correo electrónico
          tipo: cadena
          requerido: verdadero
        - nombre: tipo_instructor
          tipo: enumeración
          requerido: verdadero
          valor_permitido: "personal|contratista"
    - nombre: horario
      Objetivo: "Sesión de formación planificada"
      atributos:
        - nombre: identificación
          tipo: cadena
          requerido: verdadero
        - nombre: aula_id
          tipo: cadena
          requerido: verdadero
        - nombre: estudiante_grupo_id
          tipo: cadena
          requerido: verdadero
        - nombre: instructor_id
          tipo: cadena
          requerido: verdadero
        - nombre: día
          tipo: fecha
          requerido: verdadero
        - nombre: hora_inicio
          tipo: tiempo
          requerido: verdadero
        - nombre: hora_final
          tipo: tiempo
          requerido: verdadero
        - nombre: estado
          tipo: enumeración
          requerido: verdadero
          permitido_valor: "planificado|cancelado"
    - nombre: observación
      Propósito: "Nota de seguimiento adjunta a un cronograma"
      atributos:
        - nombre: identificación
          tipo: cadena
          requerido: verdadero
        - nombre: Schedule_id
          tipo: cadena
          requerido: verdadero
        - nombre: nota
          tipo: cadena
          requerido: verdadero
        - nombre: creado_at
          tipo: fecha y hora
          requerido: verdadero
  relaciones:
    - nombre: horario_clase
      desde: horario
      a: aula
      tipo: muchos_a_uno
    - nombre: horario_grupo_estudiante
      desde: horario
      a: grupo_estudiante
      tipo: muchos_a_uno
    - nombre: horario_instructor
      desde: horario
      a: instructor
      tipo: muchos_a_uno
    - nombre: horario_observación
      de: observación
      a: programar
      tipo: muchos_a_uno
  campos_pii:
    - entidad: instructor
      campo: número_documento
      clasificación: identificador_personal
    - entidad: instructor
      campo: correo electrónico
      clasificación: contacto
  reglas_retencion:
    - "los registros de horarios y observaciones se conservan durante el período académico activo y se archivan posteriormente".
    - "Los campos de contacto del instructor se conservan sólo mientras sean necesarios para la coordinación del horario".
    - "los datos de las semillas deben ser sintéticos y no deben contener datos personales reales".
  patrones_de consulta:
    - "listar aula ordenada por código"
    - "lista grupo_estudiantes ordenado por código"
    - "lista de instructores ordenados por nombre_completo"
    - "listar horario por día y Classroom_id"
    - "listar horario por día e instructor_id"
    - "lista de observaciones por Schedule_id"
  puntuación de calidad: 0,92
```