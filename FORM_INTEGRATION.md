# 📧 Guía de Integración del Formulario de Contacto

El formulario actual es un ejemplo básico con validación del lado del cliente. Aquí tienes varias opciones para conectarlo a un backend real.

---

## Opción 1: Formspree (Más Fácil - 5 minutos)

### Ventajas
✅ Gratis hasta 50 envíos/mes
✅ No requiere backend
✅ Setup en 5 minutos
✅ Spam protection incluido

### Pasos:
1. Ve a [formspree.io](https://formspree.io)
2. Crea una cuenta gratuita
3. Crea un nuevo formulario
4. Copia tu Form ID

### Implementación:
En `components/CTA.html`, reemplaza el formulario con:

```html
<form action="https://formspree.io/f/TU_FORM_ID" method="POST" class="space-y-5">
  <input type="hidden" name="_subject" value="Nuevo contacto desde Tolino Labs">
  <input type="hidden" name="_next" value="https://tolinolabs.com/gracias.html">
  
  <!-- Tus campos existentes con name="" -->
  <input type="text" name="nombre" required>
  <input type="email" name="email" required>
  <!-- ... resto de campos -->
  
  <button type="submit">Enviar</button>
</form>
```

---

## Opción 2: Netlify Forms (Si despliegas en Netlify)

### Ventajas
✅ Completamente gratis
✅ 100 envíos/mes
✅ Integrado con Netlify
✅ Notificaciones por email

### Pasos:
1. Despliega tu sitio en Netlify
2. Añade `data-netlify="true"` al formulario

### Implementación:
En `components/CTA.html`:

```html
<form name="contact" method="POST" data-netlify="true" data-netlify-honeypot="bot-field" class="space-y-5">
  <!-- Campo anti-spam oculto -->
  <p class="hidden">
    <label>Don't fill this out: <input name="bot-field"></label>
  </p>
  
  <!-- Tus campos existentes -->
  <input type="text" name="nombre" required>
  <input type="email" name="email" required>
  <!-- ... resto de campos -->
  
  <button type="submit">Enviar</button>
</form>
```

---

## Opción 3: Google Forms (Gratis e Ilimitado)

### Ventajas
✅ Totalmente gratis
✅ Ilimitados envíos
✅ Respuestas en Google Sheets
✅ No requiere código

### Pasos:
1. Crea un Google Form
2. Copia el enlace de "enviar"
3. Úsalo como action del formulario

### Implementación:
```html
<form action="https://docs.google.com/forms/d/e/TU_FORM_ID/formResponse" method="POST" target="hidden_iframe" onsubmit="submitted=true;">
  <input type="text" name="entry.123456789" placeholder="Nombre">
  <input type="email" name="entry.987654321" placeholder="Email">
  
  <button type="submit">Enviar</button>
</form>

<iframe name="hidden_iframe" id="hidden_iframe" style="display:none;" onload="if(submitted) {window.location='gracias.html';}"></iframe>
<script>var submitted=false;</script>
```

---

## Opción 4: EmailJS (Sin Backend)

### Ventajas
✅ 200 emails/mes gratis
✅ Envía desde Gmail, Outlook, etc.
✅ Templates personalizables
✅ Sin backend necesario

### Pasos:
1. Ve a [emailjs.com](https://emailjs.com)
2. Crea cuenta y configura servicio de email
3. Copia tus IDs

### Implementación:
Añade en el `<head>` de `index.html`:
```html
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
<script>
  (function(){
    emailjs.init("TU_PUBLIC_KEY");
  })();
</script>
```

En `components/CTA.html`, reemplaza la función `handleFormSubmit`:
```javascript
function handleFormSubmit(event) {
  event.preventDefault();
  
  emailjs.sendForm('TU_SERVICE_ID', 'TU_TEMPLATE_ID', event.target)
    .then(function() {
      alert('¡Mensaje enviado con éxito!');
      event.target.reset();
    }, function(error) {
      alert('Error al enviar: ' + error.text);
    });
  
  return false;
}
```

---

## Opción 5: Zapier/Make Webhook (Automatización Avanzada)

### Ventajas
✅ Conecta con 3000+ apps
✅ Automatizaciones complejas
✅ Envía a CRM, Slack, Email, etc.
✅ Sin límites de personalización

### Pasos:
1. Crea una cuenta en [Zapier](https://zapier.com) o [Make](https://make.com)
2. Crea un nuevo Zap/Scenario
3. Usa "Webhooks" como trigger
4. Copia la URL del webhook

### Implementación:
En `components/CTA.html`, reemplaza la función `handleFormSubmit`:
```javascript
async function handleFormSubmit(event) {
  event.preventDefault();
  
  const formData = new FormData(event.target);
  const data = Object.fromEntries(formData);
  
  try {
    const response = await fetch('TU_WEBHOOK_URL', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(data)
    });
    
    if (response.ok) {
      alert('¡Gracias! Te contactaremos pronto.');
      event.target.reset();
    } else {
      alert('Error al enviar. Inténtalo de nuevo.');
    }
  } catch (error) {
    alert('Error de conexión. Inténtalo más tarde.');
  }
  
  return false;
}
```

---

## Opción 6: Backend Personalizado (Node.js)

### Para desarrolladores que quieren control total

### Implementación básica con Express + Nodemailer:

**Backend (server.js):**
```javascript
const express = require('express');
const nodemailer = require('nodemailer');
const cors = require('cors');

const app = express();
app.use(cors());
app.use(express.json());

const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: 'tu@email.com',
    pass: 'tu_password_app'
  }
});

app.post('/api/contact', async (req, res) => {
  const { nombre, email, telefono, empresa, mensaje } = req.body;
  
  const mailOptions = {
    from: email,
    to: 'hola@tolinolabs.com',
    subject: `Nuevo contacto de ${nombre} - ${empresa}`,
    html: `
      <h2>Nuevo mensaje de contacto</h2>
      <p><strong>Nombre:</strong> ${nombre}</p>
      <p><strong>Email:</strong> ${email}</p>
      <p><strong>Teléfono:</strong> ${telefono}</p>
      <p><strong>Empresa:</strong> ${empresa}</p>
      <p><strong>Mensaje:</strong> ${mensaje}</p>
    `
  };
  
  try {
    await transporter.sendMail(mailOptions);
    res.json({ success: true });
  } catch (error) {
    res.status(500).json({ success: false, error: error.message });
  }
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

**Frontend (CTA.html):**
```javascript
async function handleFormSubmit(event) {
  event.preventDefault();
  
  const formData = new FormData(event.target);
  const data = Object.fromEntries(formData);
  
  try {
    const response = await fetch('http://localhost:3000/api/contact', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    
    const result = await response.json();
    
    if (result.success) {
      alert('¡Mensaje enviado con éxito!');
      event.target.reset();
    }
  } catch (error) {
    alert('Error al enviar el mensaje');
  }
  
  return false;
}
```

---

## Recomendación por Caso de Uso

| Caso de Uso | Solución Recomendada |
|-------------|---------------------|
| Landing page simple, pocos contactos | **Formspree** |
| Desplegado en Netlify | **Netlify Forms** |
| Necesitas integrar con CRM/Slack | **Zapier/Make** |
| Quieres enviar emails automáticos complejos | **EmailJS** |
| Budget $0, ilimitados envíos | **Google Forms** |
| Control total, backend propio | **Node.js** |

---

## Página de Gracias (Opcional)

Crea un archivo `gracias.html` en la raíz:

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>¡Gracias! - Tolino Labs</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gradient-to-br from-blue-600 to-purple-600 min-h-screen flex items-center justify-center">
  <div class="text-center text-white p-8">
    <div class="w-20 h-20 bg-white rounded-full flex items-center justify-center mx-auto mb-6">
      <svg class="w-10 h-10 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>
      </svg>
    </div>
    <h1 class="text-4xl font-bold mb-4">¡Mensaje Enviado!</h1>
    <p class="text-xl mb-8">Gracias por contactarnos. Te responderemos en menos de 24 horas.</p>
    <a href="/" class="inline-block px-8 py-3 bg-white text-blue-600 font-semibold rounded-lg hover:bg-gray-100 transition-all">
      Volver al Inicio
    </a>
  </div>
</body>
</html>
```

---

## Validación Avanzada (Opcional)

Para validación más robusta, considera usar:
- [Joi](https://joi.dev/) (backend)
- [Yup](https://github.com/jquense/yup) (frontend)
- [Validator.js](https://github.com/validatorjs/validator.js)

---

## Anti-Spam

### Honeypot (Recomendado)
Añade un campo oculto que los bots llenarán pero los humanos no:

```html
<input type="text" name="website" style="display:none" tabindex="-1" autocomplete="off">
```

En el backend, rechaza si `website` tiene valor.

### Google reCAPTCHA v3
1. Obtén keys en [recaptcha.google.com](https://www.google.com/recaptcha)
2. Añade al HTML:
```html
<script src="https://www.google.com/recaptcha/api.js?render=TU_SITE_KEY"></script>
```

---

## Testing

Antes de producción, prueba:
- ✅ Envío exitoso
- ✅ Validación de campos vacíos
- ✅ Validación de email inválido
- ✅ Protección anti-spam
- ✅ Mensaje de confirmación
- ✅ Email recibido correctamente

---

**¿Necesitas ayuda con la integración?**
Contacta: hola@tolinolabs.com

