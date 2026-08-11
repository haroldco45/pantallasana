# Pantalla Sana — Acuerdo Familiar de Pantalla

PWA de una sola página para que las familias **acuerden, midan y cumplan** el tiempo de pantalla de niños, niñas y adolescentes. Sin espionaje, sin instalación oculta, sin servidor.

**Desarrollada por Vibras Positivas HM — Derechos de Autor Reservados**

---

## Qué es y qué no es

| Sí hace | No hace |
|---|---|
| Genera un acuerdo firmado por padre/madre e hijo | Bloquear aplicaciones |
| Manual de uso incorporado (pestaña Guía) | Instalarse a escondidas |
| Define límites diarios y franjas (estudio, sueño, comidas) | Leer estadísticas del sistema Android |
| Cronómetro en vivo + registro manual por categoría | Rastrear ubicación GPS |
| Historial de 7 días, racha y puntos | Impedir desinstalación |
| Reporte semanal para WhatsApp | Funcionar sin consentimiento del menor |
| Aviso de tratamiento de datos Ley 1581/2012 | Enviar datos a ningún servidor |

**Todo se guarda en el dispositivo** (`localStorage`). No hay backend, no hay nube, no hay recolección remota. Esa es la posición de venta: la familia es la única responsable del tratamiento.

---

## Por qué no es un clon de Kids360

Kids360 bloquea apps y mide uso real porque es una app **nativa Android** con:

- `PACKAGE_USAGE_STATS` — estadísticas de uso
- `SYSTEM_ALERT_WINDOW` — superponerse para bloquear
- `DeviceAdminReceiver` — protección contra desinstalación
- `AccessibilityService` — detección de app en primer plano
- `RECEIVE_BOOT_COMPLETED` — arranque automático

Ninguna existe en la Web. Un PWA en Android corre dentro de un sandbox de navegador y no ve ni una sola app instalada.

Además, Google Play endureció el uso de `AccessibilityService` con aplicación desde el **28 de enero de 2026**, y exige el Formulario de Declaración de Permisos con aprobación previa. El control parental es una excepción permitida en la política, pero se revisa caso por caso.

Ver `ROADMAP-NATIVO.md` (sección abajo) para la ruta si algún día se quiere la versión con bloqueo real.

---

## Marco legal colombiano aplicado

| Norma | Cómo se aplica en la app |
|---|---|
| **Ley 1581 de 2012, Art. 7** | Datos de menores con protección especial. La app no transmite datos. |
| **Decreto 1377 de 2013, Art. 12** | Autorización del representante legal + interés superior + derecho a ser escuchado. Implementado en la pestaña **Acuerdo** con doble firma. |
| **Ley 1581 de 2012, Art. 12** | Aviso claro de finalidad, carácter facultativo e identificación del responsable, en Ajustes. |
| **Habeas Data — datos sensibles** | El documento de identidad se muestra enmascarado (`•••••1234`) y solo se revela con PIN de administrador. |
| **Constitución, Art. 44** | Interés superior del menor: el acuerdo no se firma sin la casilla de "mi hijo fue escuchado". |

> El acuerdo firmado en la app es la prueba de autorización que exige el Art. 9 de la Ley 1581. Se puede imprimir o exportar en PDF desde el navegador.

---

## Archivos

```
index.html      Aplicación completa (single-file PWA)
MANUAL.md       Manual de la familia (el mismo texto vive dentro de la app, pestaña Guía)
README.md       Este archivo
```

No requiere build, npm, ni dependencias. Solo dos fuentes desde Google Fonts (Fraunces + Outfit).

---

## Despliegue

### Netlify (recomendado para demo pública)

1. Arrastrar la carpeta a netlify.com/drop
2. Listo. HTTPS automático, service worker funcionando.

### GitHub Pages

```bash
git init
git add index.html README.md
git commit -m "Pantalla Sana v1.0"
git branch -M main
git remote add origin https://github.com/haroldco45/pantalla-sana.git
git push -u origin main
```

Luego: Settings → Pages → Branch `main` / root.

### Hostinger (vibraspositivashm.com)

Subir a `/public_html/pantalla-sana/`. Actualizar el `og:image` a la ruta real.

---

## Antes de publicar: la imagen Open Graph

El bloque de 9 metaetiquetas ya está en `index.html`, pero apunta a:

```
https://vibraspositivashm.com/img/pantalla-sana-og.jpg
```

**Hay que subir esa imagen en 1200×630 px** o el enlace se verá sin previsualización en WhatsApp y Facebook. Sugerencia de contenido: el sello del acuerdo sobre fondo `#0E3A46` con el texto "Pantalla Sana — Acuerdo Familiar".

---

## Modelo de negocio sugerido

Kids360 cobra suscripción por el bloqueo. Tú no tienes bloqueo, así que no compitas ahí.

**Vender el acuerdo, no la vigilancia.**

| Canal | Propuesta |
|---|---|
| Colegios de Caucasia | Licencia institucional: el colegio entrega el acuerdo a las familias como material de escuela de padres. Venta a rectoría, no a padres uno por uno. |
| Parroquia La Santísima Trinidad | Taller de familia + app gratuita como herramienta. Genera reputación y referidos. |
| Psicólogos y pediatras locales | Herramienta de tarea entre consultas. Ellos la recomiendan, tú cobras licencia anual. |
| Padres directos | Gratis. Es el gancho para los tres canales anteriores. |

El diferenciador defendible: **ninguna app internacional entrega el acuerdo redactado según el Decreto 1377 colombiano.** Kids360 es israelí/británica y su cumplimiento es RGPD, no Ley 1581.

---

## ROADMAP-NATIVO (si algún día se quiere bloqueo real)

Esto **no** es un fin de semana. Estimado realista: 3 a 5 meses de trabajo a tiempo parcial.

1. **App hija en Kotlin** — `UsageStatsManager` para estadísticas, `ForegroundService` persistente, overlay de bloqueo con `SYSTEM_ALERT_WINDOW`.
2. **App padre** — puede seguir siendo esta PWA, conectada por API.
3. **Backend de sincronización** — Node.js/Express (tu stack), pero **fuera** de la red de Distrileco. Datos de menores no pueden vivir en un servidor comercial.
4. **Registro de base de datos ante la SIC** — obligatorio para responsables del tratamiento. La versión actual lo evita justamente porque no hay servidor.
5. **Formulario de Declaración de Permisos en Play Console** — describir el uso de Accessibility en la ficha de la tienda, requisito explícito de la política.
6. **Política de privacidad publicada** — Google la exige y debe reflejar con exactitud lo que se recolecta.

Riesgo principal: el rechazo en Play Store es común en esta categoría y puede tardar semanas por iteración.

---

## Registro de cambios

**v1.1** — 11 de agosto de 2026
Pestaña **Guía** con el manual de la familia en 13 secciones desplegables e imprimibles.
Corrección de `og:image` y `og:url` al dominio de despliegue, más `og:image:width/height`.

**v1.0** — 11 de agosto de 2026
Versión inicial. Acuerdo con doble firma, límites por día, franjas horarias, cronómetro, historial de 7 días, puntos y racha, reporte para WhatsApp, respaldo JSON, PIN de administrador, reloj Colombia (UTC-5).

---

**Vibras Positivas HM** · Caucasia, Antioquia
haroldco45@gmail.com · 311 770 0431
