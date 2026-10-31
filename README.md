
# 🎓 UniEmpleos — Portal Universitario de Empleos, Pasantías y Talentos

**UniEmpleos** es una plataforma web Full-Stack diseñada para conectar a estudiantes universitarios en búsqueda de su primera experiencia profesional o pasantía con empresas y reclutadores. El sistema gestiona perfiles académicos documentados, ofertas laborales, seguimiento de postulaciones con confirmación de lectura y mensajería directa bidireccional.

---

## 🚀 Características Principales

* **Autenticación y Control de Acceso Basado en Roles (RBAC):**
  * **🎓 Rol Estudiante:** Registro con carga validada de foto de perfil (`.jpg`/`.png`), Curriculum Vitae (`.pdf`) y Certificado de Alumno Regular. Búsqueda de ofertas con filtros en vivo, postulación en un clic, panel de seguimiento con indicador de lectura (`👁️ Visto por la empresa` con fecha/hora) y edición de perfil/documentos.
  * **🏢 Rol Empresa:** Registro corporativo con carga de logo institucional, publicación y baja de ofertas laborales, gestión de candidatos postulados con cambio de estado (`Pendiente`, `En revisión`, `Entrevista`, `Aceptado`, `Rechazado`) y buscador avanzado de talentos universitarios por carrera, universidad o habilidades.
  * **🛡️ Rol Administrador:** Dashboard global con métricas en tiempo real y control total (auditoría y baja en cascada de estudiantes, empresas, ofertas y postulaciones).
* **💬 Chat Bidireccional Integrado:** Sistema de mensajería directa entre empresas y estudiantes para coordinar entrevistas sin salir de la plataforma.
* **✨ Interfaz Dinámica (SPA):** Carruseles automáticos de movimiento continuo para ofertas destacadas y empresas vinculadas, con pausa inteligente al interactuar.

---

## 🛠️ Tecnologías Utilizadas

* **Frontend:** HTML5, CSS3 (Grid, Flexbox, Diseño Responsivo, Modales nativos), JavaScript Vanilla (ES6+, Fetch API, DOM dinámico).
* **Backend:** Node.js, Express.js (API RESTful).
* **Gestión de Archivos:** Multer (almacenamiento en servidor con filtros de seguridad por extensión y limpieza física de archivos al eliminar registros).
* **Base de Datos:** MySQL (alojada en la nube mediante Filess.io), conectada con pool de promesas vía `mysql2/promise`.

---

## 🗄️ Modelo Relacional de la Base de Datos

El esquema se encuentra normalizado en **Tercera Forma Normal (3FN)** e integra 6 tablas relacionales:

1. **`administradores`**: `id_admin` (PK), `nombre`, `email` (UNIQUE), `password`, `fecha_registro`.
2. **`estudiantes`**: `id_estudiante` (PK), `nombre_completo`, `institucion`, `carrera`, `direccion`, `email` (UNIQUE), `password`, `telefono`, `linkedin_url`, `github_portfolio_url`, `instagram_url`, `descripcion`, `foto_perfil_url`, `cv_pdf_url`, `certificado_regular_url`, `fecha_registro`.
3. **`empresas`**: `id_empresa` (PK), `razon_social`, `cuit` (UNIQUE), `email_contacto` (UNIQUE), `password`, `telefono`, `rubro`, `ubicacion`, `descripcion_empresa`, `logo_url`, `fecha_registro`.
4. **`ofertas`**: `id_oferta` (PK), `id_empresa` (FK -> `empresas`), `titulo`, `descripcion`, `carrera_objetivo`, `modalidad`, `tipo_puesto`, `activa`, `fecha_publicacion`.
5. **`postulaciones`** *(Tabla intermedia N:M)*: `id_postulacion` (PK), `id_oferta` (FK -> `ofertas`), `id_estudiante` (FK -> `estudiantes`), `estado`, `leido_por_empresa`, `fecha_lectura`, `fecha_postulacion`. Restricción `UNIQUE (id_oferta, id_estudiante)`.
6. **`mensajes`**: `id_mensaje` (PK), `id_estudiante` (FK -> `estudiantes`), `id_empresa` (FK -> `empresas`), `remitente`, `contenido`, `leido`, `fecha_envio`.

---

## 📂 Estructura del Proyecto

```text
UniEmpleos/
├── public/
│   ├── index.html        # Aplicación principal unificada (SPA Multi-Rol)
│   └── styles.css        # Hojas de estilo base
├── uploads/              # Almacenamiento local de fotos, logos, CVs y certificados
├── db.example.js         # Plantilla de configuración de conexión MySQL
├── db.js                 # Conexión activa a MySQL (ignorado en Git por seguridad)
├── schema.sql            # Script DDL de creación de tablas e inicialización
├── server.js             # Servidor Node.js + Endpoints de la API REST
├── .gitignore            # Exclusión de node_modules, uploads y credenciales
└── package.json          # Dependencias y metadatos del proyecto