## Update: Examen [Número]

### Objetivo
Agregar al bot un reporte que permita a la coordinación académica ver, sin revisar registros manualmente, cuántas tutorías se realizaron por materia durante la semana en curso.

### Nodos agregados
- **TUT_Entrada_Menu_Coordinacion**: punto de conexión con el botón "Resumen de Tutorías" del menú Coordinación existente.
- **TUT_Validar Coordinador**: nodo IF que compara el `telegram_user` que disparó la acción contra el ID configurado del coordinador (`COORDINADOR_TELEGRAM_ID`). Si no coincide, se responde con un mensaje de acceso denegado y el flujo termina ahí.
- **TUT_Leer Sheet Tutorias**: nodo Google Sheets que lee todos los registros de la hoja `TUTORIAS`.
- **TUT_Procesar Tutorias**: nodo Code que:
  1. Calcula el rango de la semana actual (lunes 00:00 a domingo 23:59).
  2. Filtra los registros cuya columna `fecha` cae dentro de ese rango.
  3. Agrupa y cuenta los registros filtrados por la columna `materia`, acumulando también el total general.
- **TUT_Formatear Mensaje**: nodo Code que arma el texto final con el formato solicitado (Matemáticas, Programación, Química y total), agregando cualquier otra materia adicional que exista en la hoja como líneas extra, sin romper el formato base.
- **TUT_Enviar Resumen**: nodo Telegram que envía el mensaje formateado al chat del coordinador.

### Lógica de negocio
- El filtrado por semana se hace en código (no en la consulta a Sheets) para no depender de fórmulas o filtros previos en la hoja de cálculo, haciendo el reporte robusto ante cambios de estructura.
- El agrupado se implementó con un nodo Code en lugar de Item Lists (Summarize) para poder combinar en un mismo paso el filtro por semana y el conteo por materia, reduciendo la cantidad de nodos.
- La validación de coordinador se hace ANTES de leer la hoja, para evitar consultas innecesarias a Google Sheets si el usuario no tiene permisos.
- Las materias no contempladas explícitamente en el formato del examen (Matemáticas/Programación/Química) igual se muestran, para que el reporte no oculte información si la hoja crece con nuevas materias.

### Configuración necesaria antes de usar
| Placeholder | Dónde | Qué poner |
|---|---|---|
| `REEMPLAZAR_CON_SPREADSHEET_ID` | TUT_Leer Sheet Tutorias | ID real del spreadsheet de Google |
| `REEMPLAZAR_CREDENCIAL_SHEETS` | TUT_Leer Sheet Tutorias | Credencial de Google Sheets ya usada en el resto del workflow |
| `REEMPLAZAR_CREDENCIAL_TELEGRAM` | TUT_Acceso_Denegado, TUT_Enviar Resumen | Credencial del bot de Telegram ya usada en el resto del workflow |
| `COORDINADOR_TELEGRAM_ID` | TUT_Validar Coordinador | ID numérico de Telegram del coordinador (variable de entorno o valor fijo) |

### Pruebas realizadas
- [ ] Captura de los nodos nuevos en el canvas de n8n.
- [ ] Prueba exitosa: mensaje recibido en Telegram con el formato esperado.
- [ ] Evidencia en Google Sheets de los datos leídos.



