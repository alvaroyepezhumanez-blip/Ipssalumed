# IPS SALUMED — Agenda institucional real

Aplicación para gestionar pacientes, citas odontológicas y recordatorios, alojable en Cloudflare Pages y conectada al proyecto Supabase `kncjxucxenjvrjjtunfk`.

## Estado de seguridad y acceso

- Se **eliminó** el archivo `demo.html` y la autenticación ficticia `admin/admin`.
- El formulario inicial usa autenticación real de Supabase Auth.
- El frontend tiene la **clave de tipo publishable** de Supabase; no contiene ninguna clave `service_role`, `sb_secret_` ni tokens de WhatsApp.
- Las tablas `patients`, `appointments`, `staff` y `reminder_jobs` tienen RLS.
- El correo **promocionymantenimiento@ipssalumed.com** está preautorizado en la lista privada de la base de datos para recibir rol **admin** tras confirmar el correo en Supabase Auth.
- **La cuenta aún no se ha creado ni activado** hasta que el titular defina su propia contraseña y confirme el email. No se ha generado ni enviado una invitación desde la integración.

## Activar el primer administrador

1. Abre el sitio de Cloudflare Pages conectado a este repositorio (rama `main`).
2. En el formulario, deja el correo `promocionymantenimiento@ipssalumed.com`.
3. Escribe una **contraseña nueva y privada**, de al menos 12 caracteres con mayúsculas, minúsculas y números.
4. Selecciona **Activar cuenta por primera vez**.
5. Revisa el buzón institucional, confirma mediante el enlace recibido y vuelve a iniciar sesión.
6. Una función de base de datos añade a `public.staff` el usuario verificado preautorizado con rol `admin`. Si el correo ya existía y está verificado, el administrador puede validar la fila en la tabla `staff`.

### Configurar redirección de verificación

**Antes del paso 4**, en [Supabase Auth URL Configuration](https://supabase.com/dashboard/project/kncjxucxenjvrjjtunfk/auth/url-configuration), configura la URL real asignada por Cloudflare Pages como **Site URL** y agrégala a **Redirect URLs**. Por ejemplo, `https://TU-PROYECTO.pages.dev/**` (reemplaza por la dirección real; no uses este ejemplo como URL). En [Auth Providers](https://supabase.com/dashboard/project/kncjxucxenjvrjjtunfk/auth/providers) asegúrate de tener activada **Confirm email**. Nunca desactives la confirmación del correo para usuarios con acceso a datos de pacientes.

Para cuentas futuras, la aprobación de correos se gestiona mediante `private.staff_email_allowlist`; no concedas acceso mediante credenciales compartidas.

## Publicar con Cloudflare Pages desde GitHub

1. En Cloudflare Dashboard > Workers & Pages > Pages conecta GitHub con el repositorio `alvaroyepezhumanez-blip/Ipssalumed`.
2. Producción: rama `main`. Preset: `None`. Build command: vacío. Directorio de salida: `/`.
3. Comprueba la dirección `pages.dev` real en Cloudflare. Los cambios futuros en `main` activarán despliegues automáticos si la integración está operativa.

## Funciones

- Registrar, consultar y editar pacientes.
- Programar, buscar, editar, reprogramar y cancelar citas.
- Desactivar recordatorios sin cancelar citas.
- Consultar el estado de recordatorios programados.

**Recordatorios de WhatsApp:** solo se programan en Supabase. **No se envían mensajes reales** hasta instalar un procesador privado y conectar la API oficial, con plantilla aprobada, secretos protegidos y controles de consentimiento.

## Antes de introducir datos reales

Verifica accesos, contraseñas, HTTPS, respaldos, auditoría, funcionamiento en Cloudflare, protección de datos personales en Colombia y que cada profesional solo vea la información necesaria. Nunca uses datos de pacientes para pruebas públicas.
