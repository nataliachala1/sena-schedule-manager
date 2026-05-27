# Documento de requisitos del producto

## Visión
Proporcione una experiencia de programación confiable y libre de conflictos para los instructores del SENA.

## Objetivos
- Eliminar conflictos de programación.
- Reducir el tiempo dedicado a la creación de horarios.

## Alcance
- Gestión de instructores.
- Gestión del aula
- Definición de franja horaria
- Generación de horarios y verificación de conflictos.

## Requisitos
- El sistema DEBE evitar la doble reserva de un instructor.
- El sistema DEBE evitar la doble reserva de un aula.
- El sistema DEBE permitir el registro de observaciones para una entrada de cronograma.

## EPC y HU
- **EPC-1**: Gestión de entidades centrales
  - **FEA-1.1**: Instructor CRUD
    - HU-001: Como administrador, quiero crear un instructor.
  - **FEA-1.2**: CRUD en el aula
    - HU-002: Como administrador, quiero crear un aula.
- **EPC-2**: Programación
  - **FEA-2.1**: Asignación de horarios
    - HU-003: Como administrador, quiero asignar un instructor a un salón de clases y un horario.