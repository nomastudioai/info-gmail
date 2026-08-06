# Directorio de remitentes

Lista viva. Cuando aparezca un remitente nuevo y lo clasifiques, **agregalo acá** y
commiteá. Un remitente clasificado una vez no se vuelve a pensar nunca más.

---

## Equipo interno (nunca archivar automáticamente)

Cualquier dirección `@nomastudio.ai`. Las vistas hasta ahora:

| Dirección | Nota |
|---|---|
| `nicolas@nomastudio.ai` | Responsable. Destino de todas las derivaciones |
| `florencia.greco@nomastudio.ai` | Equipo |
| `admin@nomastudio.ai` | Cuenta administrativa del estudio |
| `all@nomastudio.ai` | Lista interna. Si info@ está solo en copia, marcar leído y no etiquetar |

Direcciones que **rebotan** y hay que avisar que están mal en alguna lista:
`noma.claw@nomastudio.ai`, `manuela.cortes@nomastudio.ai`.

## Clientes y proyectos

Los dominios detectados en la casilla. **Confirmar con Nicolás cuáles siguen activos**
antes de aplicar reglas de cliente.

| Proyecto | Dominio / seña | Estado |
|---|---|---|
| US Ophthalmic | `usophthalmic.com` | Activo, tiene reuniones semanales agendadas |
| Los Amigurumis | `theamigurumis.com`, `m.theamigurumis.com` | Activo, con SEO y Spotify propios |
| Otro Mundo | `otromundo.com.ar` | Verificado en Bing Webmaster, confirmar estado |
| Wizzy | carpeta Drive "00 Wizzy Live" | Confirmar dominio de contacto |
| Lab AI CHV | pendiente | Confirmar dominio de contacto |
| Ticketera | proyecto en Railway | Producto propio o de cliente, confirmar |

## Ruido: archivar y marcar leído siempre

| Remitente | Tipo |
|---|---|
| `noreply-dmarc-support@google.com` | DMARC |
| `noreply@dmarc.yahoo.com` | DMARC |
| `dmarcreport@microsoft.com` | DMARC |
| `no-reply@mail.nordvpn.com` | Promo |
| `news@topazlabs.com` | Newsletter |
| `em@em1.cloudflare.com` | Newsletter |
| `welcome@t.brevo.com` | Promo |
| `noreply@skool.com` | Notificación social |
| `noreply@microsoftadvertising.com` | Promo |
| `no-reply@artists.spotify.com` | Notificación social |
| `glauber@turso.tech` | Newsletter |
| `bingwb@microsoft.com` | Onboarding de producto |
| `devanshv@labaimadesimple.com` | Cold outreach |

Ojo: **`no-reply@topazlabs.com` no es lo mismo que `news@topazlabs.com`.** El primero
manda comprobantes de compra y va a Pagos y Suscripciones. El segundo es newsletter y es
ruido. Mirá el subdominio antes de archivar.

## Alertas de sistema: etiquetar `Alertas Sistema`, archivar, agrupar en el resumen

| Remitente | Servicio | Excepción que se escala |
|---|---|---|
| `hello@notify.railway.app` | Railway | Asunto con `crashed`, `failed` o `down` |
| `noreply@supabase.com` | Supabase | Advisor de seguridad crítico |
| `sa@ahrefs.com` | Ahrefs Site Audit | Health score que cae más de 20 puntos |
| `sc-noreply@google.com` | Search Console | Penalización manual o caída de indexación |
| `bingwb@microsoft.com` | Bing Webmaster | Ninguna, es ruido |

## Facturación: etiquetar `Pagos y Suscripciones`, archivar

`payments@ebanx.com`, `no-reply@account.canva.com`, `no-reply@topazlabs.com`,
y en general cualquier remitente cuyo asunto contenga factura, invoice, receipt,
comprobante, pago confirmado o payment.

## Consultas web

`onboarding@resend.dev` con asunto `CONTACTO_WEB`. Es el formulario de nomastudio.ai.

**Importante:** el remitente es Resend, no el interesado. Para contestar hay que componer
un mail nuevo a la dirección que viene en el campo Mail del cuerpo. Responder al hilo no
le llega a nadie.

Nota para mejorar más adelante: conviene configurar `Reply-To` en el formulario de la web
para que apunte al mail del interesado. Eso permitiría responder directo al hilo. Es un
cambio de una línea en el código que manda el mail por Resend.

## Nunca tocar

Remitentes de códigos de verificación y seguridad: `noreply@tm.openai.com`,
`no-reply@accounts.google.com`, y cualquier mail cuyo asunto contenga código, code, OTP,
verificación, verification o inicio de sesión.
