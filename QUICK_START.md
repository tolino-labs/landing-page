# 🚀 Guía Rápida de Inicio - Tolino Labs Landing Page

## Inicio Rápido (5 minutos)

### Paso 1: Verificar estructura
Asegúrate de tener esta estructura de archivos:
```
lding/
├── index.html
├── components/
├── styles/
└── public/
```

### Paso 2: Abrir en el navegador

**Opción A: Doble clic**
- Simplemente haz doble clic en `index.html`
- Se abrirá en tu navegador predeterminado

**Opción B: Servidor local (recomendado)**
```bash
# Con Python (si lo tienes instalado)
python3 -m http.server 8000

# Luego abre: http://localhost:8000
```

### Paso 3: ¡Listo! 🎉
Tu landing page ya está funcionando.

---

## ⚡ Personalización Rápida

### Cambiar colores principales
Busca estos valores en los componentes:
- **Azul**: `blue-600` → Cambiar por `indigo-600`, `sky-600`, etc.
- **Púrpura**: `purple-600` → Cambiar por `violet-600`, `fuchsia-600`, etc.

### Cambiar textos
Los textos están en los archivos de `/components`:
- `Hero.html` - Título principal y propuesta de valor
- `Features.html` - Descripción de servicios
- `CTA.html` - Formulario de contacto

### Añadir tu email
En `CTA.html`, busca la función `handleFormSubmit()` y modifica el comportamiento del formulario.

---

## 🎨 Integrar con Servicios Externos

### Google Analytics
1. Abre `index.html`
2. Busca `GA_MEASUREMENT_ID`
3. Reemplaza con tu ID real
4. Descomenta la sección de Analytics

### Formulario → Email
**Opción 1: Formspree (Más fácil)**
```html
<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
  <!-- Tus campos -->
</form>
```

**Opción 2: Netlify Forms**
```html
<form name="contact" method="POST" data-netlify="true">
  <!-- Tus campos -->
</form>
```

**Opción 3: Zapier/Make**
Crea un webhook y envía los datos del formulario con `fetch()`.

---

## 📱 Probar en Móvil

### Opción 1: Mismo WiFi
1. Inicia servidor local: `python3 -m http.server 8000`
2. Encuentra tu IP: `ifconfig` (Mac/Linux) o `ipconfig` (Windows)
3. En tu móvil: `http://TU_IP:8000`

### Opción 2: Tunneling (ngrok)
```bash
# Instalar ngrok
brew install ngrok  # Mac

# Iniciar
ngrok http 8000

# Te dará una URL pública temporal
```

---

## 🚀 Desplegar Online (GRATIS)

### Netlify (Recomendado - 5 minutos)
1. Ve a [netlify.com](https://netlify.com)
2. Arrastra la carpeta `lding` a la zona de drop
3. ¡Listo! Te da una URL automática

### GitHub Pages
1. Crea un repo en GitHub
2. Sube los archivos
3. Settings → Pages → Activar
4. URL: `https://tuusuario.github.io/repo`

### Vercel
1. Ve a [vercel.com](https://vercel.com)
2. Conecta tu GitHub
3. Selecciona el repo
4. Deploy automático

---

## 🐛 Solución de Problemas

### Los componentes no se cargan
**Problema**: Ves el header pero no el contenido.
**Solución**: Usa un servidor local (no abras el HTML directamente).

```bash
python3 -m http.server 8000
```

### Las rutas no funcionan
**Problema**: Rutas relativas incorrectas.
**Solución**: Asegúrate de que `index.html` está en la raíz del proyecto.

### AOS no anima
**Problema**: CDN bloqueado o no cargado.
**Solución**: Verifica la consola del navegador (F12).

---

## 📊 Optimización Post-Lanzamiento

### 1. Imágenes
- Convierte a WebP: [Squoosh.app](https://squoosh.app)
- Comprime: [TinyPNG.com](https://tinypng.com)
- Usa dimensiones exactas necesarias

### 2. Performance
- Audita con Lighthouse (Chrome DevTools)
- Meta: 90+ en todas las métricas

### 3. SEO
- Configura Google Search Console
- Envía el `sitemap.xml`
- Verifica indexación

### 4. Analytics
- Configura eventos personalizados
- Trackea clics en CTAs
- Mide conversiones del formulario

---

## 💡 Próximos Pasos

1. ✅ Añade imágenes reales en `/public/assets`
2. ✅ Configura el formulario con backend real
3. ✅ Personaliza colores y textos
4. ✅ Añade tu dominio personalizado
5. ✅ Configura SSL/HTTPS
6. ✅ Instala Google Analytics
7. ✅ Haz pruebas A/B de CTAs

---

## 📚 Recursos Útiles

- **TailwindCSS Docs**: [tailwindcss.com/docs](https://tailwindcss.com/docs)
- **AOS Library**: [michalsnik.github.io/aos](https://michalsnik.github.io/aos/)
- **Can I Use**: [caniuse.com](https://caniuse.com)
- **PageSpeed Insights**: [pagespeed.web.dev](https://pagespeed.web.dev)

---

## 🆘 Soporte

¿Necesitas ayuda? Contacta:
- **Email**: hola@tolinolabs.com
- **LinkedIn**: [Tolino Labs](https://www.linkedin.com/company/tolino-labs)

---

**¡Éxito con tu landing page! 🚀**

