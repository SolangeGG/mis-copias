# Stock Mis Copias

Control de stock, ventas y fiados de Mis Copias. La página se publica en GitHub Pages y los datos se guardan en Supabase.

## Archivos

- `index.html`: la página.
- `logo.jpg`: el logo (ícono de la pestaña).
- `supabase.sql`: crea la base de datos con sus permisos.

---

## Paso 1 · Crear la base en Supabase (una sola vez)

1. Entrá a https://supabase.com, tocá **Start your project** y creá una cuenta.
2. Tocá **New project**. Nombre: `mis-copias`. Elegí una contraseña de base de datos (guardala) y la región **South America (São Paulo)**. Tocá **Create new project** y esperá un par de minutos.
3. En el menú de la izquierda, entrá a **SQL Editor** → **New query**. Pegá todo el contenido de `supabase.sql` y tocá **Run**. Tiene que decir *Success*.
4. Entrá a **Authentication** → **Sign In / Providers** (o **Providers**) → **Email** y desactivá **Confirm email**. Así no hace falta confirmar mails.
5. Entrá a **Authentication** → **Users** → **Add user** → **Create new user** y creá los 3 usuarios, marcando **Auto Confirm User**:
   - `nina@miscopias.com`, con la contraseña de Nina
   - `ruth@miscopias.com`, con la contraseña de Ruth
   - `aby@miscopias.com`, con la contraseña de Aby

   Los mails pueden ser inventados: solo sirven para entrar. Si usás otros, cambialos también en `index.html` (ver Paso 2).
6. Volvé a **SQL Editor** → **New query** y ejecutá esto para que Nina sea la dueña:
   ```sql
   update public.perfiles set rol = 'duena', nombre = 'Nina'
     where id = (select id from auth.users where email = 'nina@miscopias.com');
   ```
7. Entrá a **Project Settings** → **API** (o **Data API** / **API Keys**) y copiá dos datos:
   - **Project URL**, que tiene la forma `https://xxxx.supabase.co`
   - la clave **anon public**

## Paso 2 · Completar la configuración en `index.html`

Abrí `index.html` con el Bloc de notas o VS Code, buscá `const CONFIG` (está casi al final) y reemplazá:

```js
supabaseUrl: "PEGAR-AQUI-PROJECT-URL",
supabaseKey: "PEGAR-AQUI-ANON-PUBLIC-KEY",
```

con los dos datos que copiaste. Si los mails de los usuarios son otros, cambialos en `usuarios`.

> La clave *anon public* puede estar en una página pública. Lo que protege los costos son los permisos de la base (`supabase.sql`), no esa clave.

## Paso 3 · Publicar en GitHub Pages

1. En GitHub, tocá **New repository**. Nombre: `mis-copias`. Marcalo **Public**, porque GitHub Pages gratis necesita repositorio público. Los datos no quedan públicos: están en Supabase y piden usuario y contraseña. Tocá **Create repository**.
2. Tocá **uploading an existing file** y subí `index.html`, `logo.jpg` y este `README.md`. Tocá **Commit changes**. No subas `supabase.sql` si no querés, porque no hace falta para que funcione.
3. Entrá a **Settings** → **Pages**. En **Source** elegí **Deploy from a branch**, rama **main**, carpeta **/ (root)**, y tocá **Save**.
4. En uno o dos minutos la página queda en `https://TU-USUARIO.github.io/mis-copias/`.

## Paso 4 · En cada compu

- Abrí el link en Chrome y guardalo en Favoritos, o creá un acceso directo: ⋮ → *Transmitir, guardar y compartir* → *Crear acceso directo* (marcá *Abrir como ventana*).
- **Compu del negocio**: Ruth y Aby entran cada una con su contraseña. Al terminar el turno, cada una toca **Cerrar sesión**.
- **Nina** entra desde su compu con su usuario y ve todo: costos, proveedores, compras y anulaciones.

## Cuando estés lista

Entrá como Nina → **Inventario** → **Borrar ejemplos**, y cargá los productos reales.

## Para cambiar una contraseña o agregar una vendedora

Supabase → **Authentication** → **Users**. Para una vendedora nueva, creá el usuario (por ejemplo `sofi@miscopias.com`) y agregala en `usuarios` dentro de `index.html`. El nombre que se muestra sale de la primera parte del mail: `sofi@...` aparece como "Sofi".
