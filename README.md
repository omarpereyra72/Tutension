# TuTensión (versión web · datos en el navegador)

Registro de tensión arterial, pulso, peso, IMC y análisis clínicos. Sitio estático de un solo archivo: **sin servidor, sin cuenta, sin build**.

- **Datos:** se guardan en el `localStorage` del navegador del usuario. Nada sale del dispositivo.
- **Copia de seguridad:** *Perfil → Mis datos* permite **exportar** (JSON) e **importar** (combinar o reemplazar). La app avisa en Inicio cuando conviene exportar.
- **Instalable:** funciona como app en el celular (menú del navegador → *Agregar a pantalla de inicio*) y sin conexión.
- **Pantallas:** vista celular y vista PC (automática o manual), tema claro/oscuro.

> Esta es la versión **independiente** de la app de Google Apps Script/Sheets: misma lógica, mismas reglas clínicas y mismo diseño, pero los datos viven en cada navegador en vez de en una planilla.

## Estructura

```
index.html              ← toda la app (HTML + CSS + JS)
sw.js                   ← service worker (modo sin conexión)
manifest.webmanifest    ← datos de instalación (PWA)
icon.svg, icon-192.png, icon-512.png
vercel.json             ← cabeceras para Vercel
```

## Subir a GitHub

```bash
git init
git add .
git commit -m "TuTensión: versión web con localStorage"
git branch -M main
git remote add origin https://github.com/<tu-usuario>/tutension.git
git push -u origin main
```

## Publicar en Vercel

1. En <https://vercel.com/new> elegí **Import Git Repository** y seleccioná el repo.
2. *Framework Preset:* **Other**. Dejá vacíos *Build Command* y *Output Directory*.
3. **Deploy**. Cada `git push` a `main` vuelve a publicar solo.

(Alternativa sin GitHub: `npx vercel --prod` dentro de la carpeta.)

También sirve **GitHub Pages**: *Settings → Pages → Deploy from a branch → main / root*.

## Notas importantes

- El `localStorage` es **por navegador y por dominio**: si cambiás de dominio (por ejemplo del `.vercel.app` a uno propio) o de dispositivo, hay que exportar e importar.
- Borrar «datos del sitio» o navegar en modo privado elimina o no conserva los datos. Exportá copias periódicas.
- Cuando cambies `index.html`, subí también el número de `CACHE` en `sw.js` (`tutension-v1` → `v2`) si querés forzar la actualización en dispositivos ya instalados.

## Cómo escalar

Todo el almacenamiento está en el bloque `ALMACENAMIENTO LOCAL` de `index.html` (objeto `LOCAL` + `tx()`): la interfaz solo llama a `api(...)`. Para pasar a una base de datos real (Supabase, Firebase, etc.) alcanza con reemplazar ese bloque manteniendo los mismos nombres de funciones.

## Aviso

TuTensión es una herramienta de registro y orientación. No reemplaza la consulta médica ni sirve para diagnosticar.
