# 🐾 MisGatos — PWA

App para gestionar los gatos de casa. Funciona como PWA instalable desde el iPhone.

## Instalación local

```bash
npm install
npm start
```

## Deploy en Vercel (gratis)

### Opción A — Desde GitHub (recomendado)

1. Sube este proyecto a un repo en GitHub:
   ```bash
   git init
   git add .
   git commit -m "feat: MisGatos PWA inicial"
   git remote add origin https://github.com/TU_USUARIO/misgatos.git
   git push -u origin main
   ```

2. Ve a [vercel.com](https://vercel.com) → **New Project**
3. Importa el repo de GitHub
4. Configuración automática (detecta React) → **Deploy**
5. Vercel te da una URL del tipo `misgatos.vercel.app`

### Opción B — Vercel CLI

```bash
npm install -g vercel
npm run build
vercel --prod
```

---

## Instalar en iPhone como app

1. Abre la URL en **Safari** (importante, no Chrome)
2. Pulsa el botón **Compartir** ↑ (cuadrado con flecha)
3. Toca **"Añadir a pantalla de inicio"**
4. Ponle nombre → **Añadir**
5. ¡Ya tienes el icono 🐾 en tu pantalla de inicio!

---

## Credenciales demo

- Usuario: `miguel`
- Contraseña: `gatos123`

Los gatos se guardan en `localStorage` del dispositivo.
