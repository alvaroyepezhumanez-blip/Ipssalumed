# IPS SALUMED — Agenda de citas

Aplicación estática para Cloudflare Pages conectada a Supabase. Incluye acceso de personal autorizado, pacientes, citas, edición, cancelación y consulta de recordatorios programados.

## Paso previo: configurar la clave pública de Supabase

La URL del proyecto Supabase está configurada en `index.html`, pero la clave pública se dejó como `PON_AQUI_TU_CLAVE_PUBLICABLE` para no transferir credenciales mediante esta integración. **Antes de usar la web**, en Supabase Dashboard > Project Settings > API Keys, copia la clave de tipo *Publishable* del proyecto `kncjxucxenjvrjjtunfk`. En GitHub abre `index.html`, pulsa Edit (lápiz), sustituye exactamente `PON_AQUI_TU_CLAVE_PUBLICABLE` por la clave `sb_publishable_...` y confirma el cambio en la rama `main`. La clave *publishable* puede estar en el frontend; **nunca uses `service_role`, `sb_secret_` ni tokens de WhatsApp**.

## Publicar en Cloudflare Pages

1. Cloudflare Dashboard > Workers & Pages > Create > Pages > Connect to Git.
2. Autoriza GitHub y selecciona `alvaroyepezhumanez-blip/Ipssalumed`, rama `main`.
3. Framework preset: `None`. Build command: sin comando. Build output directory: `/` (raíz del repositorio).
4. Publica y comprueba que aparezca la pantalla de acceso.
5. Opcional: configura dominio `agenda.ipssalumed.com` y URL permitida en Supabase Authentication.

Cloudflare desplegará automáticamente nuevos commits a `main`.

## Habilitar acceso de personal

En Supabase Dashboard > Authentication > Users crea o invita al primer usuario. Luego ejecuta desde SQL Editor, reemplazando el UUID por el identificador real:

```sql
insert into public.staff(user_id,display_name,role)
values ('UUID_REAL','Administrador IPS SALUMED','admin');
```

Los usuarios autenticados sin registro activo en `staff` no pueden entrar a la agenda. Las tablas cuentan con RLS. No introduzcas datos reales hasta realizar pruebas de acceso, copia de seguridad, auditoría y cumplimiento de protección de datos.

## Recordatorios WhatsApp

La base de datos programa avisos a 24 h y 2 h de la cita si hay autorización del paciente. **Los mensajes aún NO se envían**: falta incorporar una integración de servidor con WhatsApp Business Platform, plantilla aprobada, programador, gestión de reintentos y webhook de confirmación. No publiques información clínica en los mensajes.

## Archivos

- `index.html`: interfaz y llamadas a Supabase.
- `_headers`: cabeceras defensivas de Cloudflare.
- `README.md`: esta guía.

> Nota: El repositorio es público; los datos de pacientes permanecen exclusivamente en Supabase, no en GitHub.
