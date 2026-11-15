# IPS SALUMED — Agenda institucional real

Aplicación para gestionar pacientes, citas odontológicas y recordatorios, alojable en Cloudflare Pages y conectada al proyecto Supabase `kncjxucxenjvrjjtunfk`.

## Estado de seguridad y acceso

- Se **eliminó** el archivo `demo.html` y la autenticación ficticia `admin/admin`.
- El formulario inicial usa autenticación real de Supabase Auth.
- El frontend tiene la **clave de tipo publishable** de Supabase; no contiene ninguna clave `service_role`, `sb_secret_` ni tokens de WhatsApp.
- Las tablas `patients`, `appointments`, `staff` y `reminder_jobs` tienen RLS.
- El correo **promocionymantenimiento@ipssalumed.com** está preautorizado en la lista privada de la base de datos para recibir rol **admin** tras confirmar el correo en Supabase Auth.
- **La cuenta aún no se ha creado ni activado** hasta que el titular defina su propia contraseña y confirme el email. No se ha generado ni enviado una invitación desde la integración.

## Registro desde la página

En la página principal, selecciona **Crear una cuenta**, escribe cualquier correo que controles, crea una contraseña de al menos 8 caracteres, sin requisitos de mayúsculas, números ni caracteres especiales, repítela y pulsa **Registrarme**. El formulario utiliza `supabase.auth.signUp` y no concede privilegios clínicos automáticamente.

**Verificación de correo:** Supabase puede exigir un mensaje de confirmación. Antes de operar en producción, configura [SMTP personalizado](https://supabase.com/dashboard/project/kncjxucxenjvrjjtunfk/auth/smtp) y registra la dirección de Cloudflare Pages en [Auth URL Configuration](https://supabase.com/dashboard/project/kncjxucxenjvrjjtunfk/auth/url-configuration). El SMTP predeterminado de Supabase puede rechazar correos de personas que no pertenecen al equipo del proyecto. Por ello, publicar el formulario **no garantiza** que ya lleguen los emails ni que el registro se complete sin configurar el correo.

Una cuenta nueva puede autenticarse después de verificar el correo, pero **no puede acceder a pacientes o citas** hasta ser autorizada en `public.staff`. El correo `promocionymantenimiento@ipssalumed.com` ya está preautorizado para obtener rol `admin` **solo después de verificar su identidad por correo**. Los demás correos necesitan aprobación individual por un administrador, sin dar acceso por defecto.

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
