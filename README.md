# TutorBot – Sistema de Asesorías Académicas

**Repositorio:** 
Tutor-Boot

**Integrantes:**
- Stefani Fabiola Sánchez Pérez
- Irma Yaneth Arias García

---

##  Introducción

En el entorno educativo actual, la coordinación de asesorías académicas suele ser un proceso caótico y manual. Los estudiantes dependen de correos electrónicos o mensajes informales para encontrar un tutor, mientras que los tutores no tienen una agenda centralizada para gestionar su disponibilidad. Este proceso genera cruces de horarios, desatención de materias críticas y falta de trazabilidad.

**TutorBot** es una solución automatizada desarrollada en **n8n** que conecta a estudiantes con tutores mediante un motor de asignación inteligente. El sistema gestiona desde la solicitud inicial hasta la finalización de la asesoría, garantizando que cada materia cuente con el tutor adecuado en el horario disponible, optimizando así el recurso humano y mejorando el rendimiento académico.

---

##  Objetivos

- Desarrollar un sistema automatizado para la gestión de tutorías académicas que integre Telegram, Google Sheets y lógica avanzada de asignación.
- Implementar un motor de búsqueda que asocie automáticamente materia, tutor y horario libre.
- Diseñar una interfaz conversacional en Telegram para la autogestión del estudiante (solicitar, consultar, cancelar).
- Automatizar el control de estados de la tutoría (Solicitada, Asignada, Confirmada, Finalizada).
- Generar reportes automáticos de actividad para la coordinación académica.
- Validar disponibilidad en tiempo real para evitar cruces de agenda o doble reserva.

---

##  Descripción del Sistema

El sistema está compuesto por los siguientes módulos:

### 1. Interfaz en Telegram
Punto de contacto único donde el usuario puede:
- Registrarse como estudiante.
- Seleccionar materias mediante menús numéricos.
- Consultar el estado de sus solicitudes.
- Recibir recordatorios automáticos de sus citas.

### 2. Motor de Automatización (n8n)
Orquestador que procesa la lógica del negocio:
- **Gestión de Sesiones:** mantiene el estado del usuario para guiarlo en flujos de varios pasos (Wizard).
- **Lógica de Asignación:** busca en la base de datos tutores que coincidan con la materia y tengan horario libre.
- **Notificaciones:** envía alertas tanto al estudiante como al tutor cuando se realiza un emparejamiento.

### 3. Modelo de Datos (Google Sheets)

Base de datos: **TutorBot_DB**

**Hoja: TUTORES**
| Campo | Descripción |
|---|---|
| id_tutor | Identificador único del tutor |
| nombre | Nombre completo del tutor |
| especialidad_materias | Materias que el tutor puede impartir |
| estado | Activo / Inactivo |

**Hoja: DISPONIBILIDAD**
| Campo | Descripción |
|---|---|
| id_dispo | Identificador único del bloque de disponibilidad |
| id_tutor | Referencia al tutor |
| dia_semana | Día de la semana disponible |
| hora_inicio | Hora de inicio del bloque |
| hora_fin | Hora de fin del bloque |
| estado | Libre / Ocupado |

**Hoja: TUTORIAS**
| Campo | Descripción |
|---|---|
| id_tutoria | Identificador único de la tutoría |
| id_estudiante | Referencia al estudiante |
| id_tutor | Referencia al tutor asignado |
| materia | Materia de la tutoría |
| fecha | Fecha programada |
| hora | Hora programada |
| estado | Solicitada / Asignada / Confirmada / Finalizada |

**Hoja: SESSIONS**
| Campo | Descripción |
|---|---|
| telegram_user | Usuario de Telegram |
| pantalla_actual | Pantalla/paso actual del flujo |
| paso_actual | Paso dentro del wizard |
| datos_parciales | Datos capturados temporalmente durante el flujo |

---

##  Flujos Guiados 

### Flujo "Solicitar Tutoría"

1. **Materia:** el bot muestra las materias disponibles.
2. **Fecha:** el usuario ingresa la fecha (`YYYY-MM-DD`).
3. **Búsqueda:** el sistema busca tutores disponibles en esa fecha.
4. **Confirmación:** el usuario confirma el tutor y horario sugerido.
5. **Notificación:** se registra la tutoría como "Asignada" y se avisa al tutor.

---

##  Resultado Esperado

1. Reducción del 90% en el tiempo de asignación de tutorías.
2. Trazabilidad total: historial completo de quién solicitó, quién atendió y cuándo terminó.
3. Escalabilidad: capacidad para gestionar cientos de tutores y estudiantes simultáneamente.
4. Experiencia de usuario: interfaz amigable que guía al estudiante paso a paso sin necesidad de manuales.

---

##  Entrega

-  Repositorio de GitHub: `Proyecto_TutorBot_SanchezPerez_AriasGarcia`
-  Integrantes del grupo: Stefani Fabiola Sánchez Pérez, Irma Yaneth Arias García
-  Archivo `README.md` con instrucciones y capturas del flujo
-  Archivo `.json` con el flujo completo de n8n
-  Acceso compartido al Google Sheets de base de datos

---

##  Tecnologías Utilizadas

- **n8n** – Motor de automatización y orquestación de flujos.
- **Telegram Bot API** – Interfaz conversacional con el usuario.
- **Google Sheets API** – Almacenamiento y gestión de la base de datos.

---

##  Autoras

| Nombre | Rol |
|---|---|
| Stefani Fabiola Sánchez Pérez | Desarrolladora |
| Irma Yaneth Arias García | Desarrolladora |
