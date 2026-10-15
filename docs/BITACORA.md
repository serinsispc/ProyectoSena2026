# Bitácora del proyecto VetCare Huila

Este registro recoge la preparación del repositorio y del alojamiento para el sistema de gestión clínica y bienestar animal. La infraestructura inicial de IIS está creada; la aplicación, la API y sus pruebas todavía están pendientes.

Fecha de registro inicial: 14 de octubre de 2026, hora de Colombia. Los registros siguientes se reconstruyen a partir de los procedimientos y resultados observados durante esta sesión.

Responsable del proyecto en esta sesión: Emiliano Polania. Ejecución asistida mediante Codex. La distribución de responsabilidades del Grupo 7 está pendiente de documentar.

## Registro 001 Preparación del repositorio

**Objetivo:** dejar la carpeta del proyecto preparada para versionar el código y la documentación.

### Procedimiento realizado

1. Revisar el estado de Git, la rama activa y el repositorio remoto.
2. Comprobar que la carpeta ya era un repositorio y tenía una confirmación inicial.
3. Crear `.gitignore` para excluir temporales de Visual Studio, resultados de compilación y archivos de configuración privada.
4. Quitar del seguimiento de Git los archivos de `.vs`, conservándolos en el equipo.
5. Crear el `README.md` inicial.
6. Guardar los cambios en una confirmación local.

### Resultado y evidencia

| Elemento | Resultado observado |
| --- | --- |
| Rama | `main` |
| Remoto | `https://github.com/serinsispc/ProyectoSena2026.git` |
| Confirmación inicial existente | `280cec9`, mensaje `crear proyecto` |
| Confirmación de preparación | `b80d013`, mensaje `Preparar repositorio de VetCare Huila` |
| Estado al finalizar ese procedimiento | Sin cambios pendientes. |

La evidencia corresponde a las salidas de Git y a los archivos `.gitignore` y `README.md`. No se ejecutó un envío a GitHub durante este procedimiento; tener un remoto configurado no acredita que la confirmación se haya publicado.

## Registro 002 Análisis del caso de estudio

**Objetivo:** comprender el documento `PROYECTO_FORMATIVO_MVP_VETERINARIA.md` antes de implementar la solución.

### Procedimiento realizado

1. Localizar y leer el documento completo.
2. Identificar las áreas de propietarios y mascotas, agenda, historia clínica, prescripciones, vacunación, inventario y seguridad.
3. Revisar las seis fases y los criterios de evaluación.
4. Señalar decisiones de negocio que necesitan aclaración.

### Resultado y pendientes

El análisis se presentó en la conversación sin modificar el documento original. Entre los puntos pendientes están la diferencia entre prescribir y consumir medicamentos, el cierre del folio clínico y las adendas, el manejo de lotes, las excepciones de cancelación y el alcance del portal del propietario.

La extracción formal de actores, historias de usuario, requisitos, reglas de negocio y matriz de trazabilidad todavía no se ha completado.

## Registro 003 Definición inicial del entorno tecnológico

**Objetivo:** establecer una propuesta compatible con el conocimiento y la infraestructura disponible del responsable del proyecto.

### Decisiones y propuestas

| Elemento | Estado |
| --- | --- |
| C# | Lenguaje elegido por el usuario. |
| SQL Server | Motor indicado por el usuario. Su versión y configuración para VetCare están pendientes de verificar. |
| Windows Server e IIS | Entorno existente, inspeccionado mediante Escritorio remoto. |
| ASP.NET Core Web API | Propuesta para los servicios del proyecto. |
| Blazor WebAssembly | Propuesta para la interfaz web. |
| .NET MAUI | Propuesta para una aplicación móvil instalada, si se confirma ese entregable. |
| Entity Framework Core | Propuesta para acceso a SQL Server. |
| Versión de .NET | Pendiente de verificar compatibilidad e instalación en el servidor antes de crear y publicar la aplicación. |

### Diferencias frente a la guía académica

La sección 3.4 de la guía pide PostgreSQL o MySQL. El uso de SQL Server debe acordarse con el instructor. La sección 3.5 pide `Dockerfile` y `docker-compose.yml`; publicar en IIS no sustituye por sí solo esos entregables.

La guía menciona interfaz web o móvil y diseño adaptable. Debe confirmarse si la evaluación exige una aplicación móvil instalada además de la web.

## Registro 004 Preparación de la aplicación en IIS

**Objetivo:** reservar el alojamiento de VetCare dentro de `serinsispc.com/proyectosena2026`, con un grupo de aplicaciones propio.

### Entorno observado

| Elemento | Valor |
| --- | --- |
| Sistema operativo | Microsoft Windows Server 2019 Standard |
| Servidor web | IIS 10 |
| Sitio padre | `SerinsisPC` |
| Enlaces del sitio padre | HTTP y HTTPS para `serinsispc.com` y `www.serinsispc.com`, visibles en IIS. |

### Procedimiento realizado

1. Acceder a la sesión existente de Escritorio remoto.
2. Consultar la información del servidor en Administrador del servidor.
3. Abrir Administrador de Internet Information Services y revisar los sitios y grupos de aplicaciones existentes.
4. Inspeccionar un sitio API existente y observar el controlador `aspNetCore` asociado a `AspNetCoreModuleV2`. Esta observación no comprueba la versión de .NET instalada ni el funcionamiento de VetCare.
5. Crear el grupo `VetCare_ProyectoSena2026`, con la opción **Sin código administrado**, canalización **Integrada** e inicio inmediato.
6. Crear en el servidor la carpeta `G:\ProyectoSena2026` mediante el Explorador de archivos.
7. En el sitio `SerinsisPC`, seleccionar **Agregar aplicación**.
8. Asignar el alias `proyectosena2026`, el grupo `VetCare_ProyectoSena2026` y la carpeta física `G:\ProyectoSena2026`.
9. Guardar la aplicación y comprobar que aparece en el árbol de IIS.
10. Abrir **Filtrado de solicitudes**, pestaña **Verbos HTTP**, y revisar la configuración efectiva mostrada.

### Resultado y evidencia

La aplicación `/proyectosena2026` se creó dentro del sitio existente. No se creó un dominio o sitio independiente. El grupo propio separa su proceso de ejecución de los grupos utilizados por otras aplicaciones.

La pestaña **Verbos HTTP** mostró una lista vacía y el aviso de que los verbos no incluidos no se bloquean. No se añadieron reglas de filtrado. Esta comprobación no descarta bloqueos por WebDAV, otros módulos, la aplicación o servicios externos.

La evidencia visual fue observada durante la sesión de control del equipo. No se han archivado capturas como archivos en este repositorio. Para la entrega académica queda pendiente guardar capturas de la aplicación en IIS, su configuración básica, el grupo propio y la pestaña de verbos HTTP.

### Rutas previstas para la publicación

| Componente | Ruta prevista |
| --- | --- |
| Aplicación web | `https://serinsispc.com/proyectosena2026/` |
| API | `https://serinsispc.com/proyectosena2026/api/` |
| Swagger UI | `https://serinsispc.com/proyectosena2026/swagger/` |
| Aplicación móvil | Se instalará en el dispositivo y consumirá la API; no se ejecuta como aplicación móvil dentro de IIS. |

Solo la aplicación base de IIS está creada. Las rutas de API y Swagger son parte de la propuesta y requieren implementar y publicar el código. No se verificó una respuesta HTTP ni la validez del certificado para esta ruta.

### Verificaciones pendientes

- Comprobar el runtime y el Hosting Bundle correspondientes a la versión elegida de .NET.
- Confirmar permisos de lectura y ejecución de la carpeta para la identidad del grupo.
- Revisar la herencia de configuración del sitio padre y el comportamiento de WebDAV para la aplicación nueva.
- Configurar las rutas base de la web, API y Swagger para funcionar bajo `/proyectosena2026`.
- Configurar el acceso a SQL Server sin guardar credenciales en Git.
- Publicar la aplicación y verificar HTTPS y conexión a la base de datos.
- Probar GET, POST, PUT, PATCH y DELETE con datos de prueba propios del proyecto.
- Documentar los resultados y corregir los problemas encontrados antes de declarar el entorno operativo.

No se modificaron los enlaces del sitio padre ni los grupos de las otras aplicaciones. No se reinició IIS ni se ejecutaron operaciones de modificación sobre datos de las APIs existentes durante esta preparación.
