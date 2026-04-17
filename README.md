# APPSADO 🔥

**Todo para tu asado, en una sola app.**  
Organizá, calculá, cociná y dividí gastos sin quilombos.

🌐 [appsado.vercel.app](https://appsado.vercel.app)

---

## ¿Qué es APPSADO?

App PWA para organizar asados en Uruguay, Argentina o donde sea. Sin registro obligatorio, funciona offline y se comparte con un código de 6 caracteres.

## Features

| Feature | Descripción |
|---|---|
| 💸 Dividir gastos | Creá un asado, cargá los gastos colaborativamente y calculá quién le debe a quién con la menor cantidad de transferencias posibles |
| 🥩 Asadómetro | Calculá cantidades exactas de carne, carbón, achuras y extras según la cantidad de personas |
| ⏱️ Cronómetro parrillero | Elegí los cortes y la app arma el cronograma para que todo salga junto |
| 🃏 Truco | Anotador de puntos con palitos (a 15, 30 o 40 puntos) |

## Tech stack

- **Frontend:** Vanilla JS + HTML/CSS (sin framework, sin build tools)
- **Base de datos / Auth:** [Supabase](https://supabase.com) (PostgreSQL + auth)
- **Autenticación:** Google OAuth + Magic Link (email)
- **PWA:** Service Worker para soporte offline
- **Librerías CDN:** Chart.js 4.4, jsPDF 2.5.1
- **Deploy:** Vercel

## Estructura

```
APPSADO/
├── index.html      # Toda la app (HTML + CSS + JS)
├── sw.js           # Service worker (cache offline)
├── manifest.json   # Configuración PWA
└── *.png           # Íconos (192, 512, favicon, apple-touch)
```

## Cómo correr en local

```bash
# Cualquier servidor HTTP estático sirve, por ejemplo:
npx serve .
# o
python3 -m http.server 8080
```

Abrí `http://localhost:8080` en el navegador.

> No hace falta build ni npm install — no hay dependencias locales.

## Variables de entorno

Las credenciales de Supabase están hardcodeadas en `index.html` (clave `anon` pública, safe para cliente). Para un fork propio:

1. Creá un proyecto en [supabase.com](https://supabase.com)
2. Reemplazá `SB` y `SK` en `index.html` con tu URL y clave anon
3. Creá las tablas `asados` y `profiles` (ver esquema abajo)

### Esquema Supabase

```sql
-- Tabla de asados
create table asados (
  code text primary key,
  data jsonb,
  user_id uuid references auth.users,
  created_at timestamptz default now()
);

-- Tabla de perfiles
create table profiles (
  id uuid primary key references auth.users,
  display_name text,
  bank_acc text,
  mp_link text,
  phone text,
  updated_at timestamptz default now()
);
```

## Flujo principal

1. El organizador crea el asado (nombre, lugar, fecha, participantes, gastos)
2. Se genera un código único de 6 caracteres
3. Los demás entran con el código desde su celu (sin registro)
4. Cualquiera puede agregar gastos colaborativamente
5. La app calcula las transferencias mínimas para saldar todo
6. Se comparte el resumen por WhatsApp o se descarga en PDF

## Licencia

Proyecto personal. Hecho con 🔥 para los asados del Río de la Plata.
