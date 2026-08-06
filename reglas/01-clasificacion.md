# Clasificación y acción por tipo de mail

Cada mail que entra cae en una de estas categorías. La acción es determinística: si el
mail encaja en la categoría, hacés la acción, sin deliberar de nuevo cada vez.

Notación:
- **Archivar** = quitar el label `INBOX`. El mail sigue existiendo y es buscable.
- **Marcar leído** = quitar el label `UNREAD`.
- **Papelera** = solo para spam evidente. Se recupera dentro de los 30 días.

---

## A. Ruido industrial

Archivar y marcar leído, sin excepción, sin reportar individualmente. En el resumen
diario solo va el total agregado.

| Subtipo | Criterio | Nota |
|---|---|---|
| Reportes DMARC | Asunto contiene `Report Domain` o `Report domain`, o remitente es `noreply-dmarc-support@google.com`, `noreply@dmarc.yahoo.com`, `dmarcreport@microsoft.com` | 3 por día. 100% automático. Solo importa si alguna vez hay un pico raro de fallos de autenticación, y eso no se lee mail por mail |
| Newsletters de producto | Remitentes de la lista de ruido en `02-remitentes.md` | |
| Promociones | Categoría `promotions` de Gmail sin relación con un cliente activo | |
| Cold outreach | Alguien que ofrece un servicio, newsletter o herramienta sin relación previa | Si insiste 3 veces o más, papelera |
| Notificaciones sociales | Skool, Spotify for Artists, LinkTree y similares | |

## B. Alertas de sistema

Etiquetar `Alertas Sistema`, archivar, marcar leído. **Van agrupadas en el resumen
diario como una sola línea**, no una por mail.

Ejemplos: Railway (deploys y parches de seguridad), Supabase (advisors), Ahrefs (site
audits), Google Search Console, Bing Webmaster Tools, Vercel.

**Excepción que rompe la regla y se escala:** si la alerta dice que algo está **caído,
crasheado o con pérdida de datos** en producción de un cliente. Ejemplo real visto:
`Deployment crashed for ticketera-2 front in Ticketera`. Eso no se archiva: se etiqueta
`ACCION REQUERIDA` y se manda push inmediato.

Distinguí bien: "actualizamos tu Postgres a un parche, no hacés nada" es rutina.
"Tu deploy crasheó" es incidente.

## C. Facturación y pagos

Etiquetar `Pagos y Suscripciones`, archivar, marcar leído. Listar en el resumen diario
con el monto y la moneda tal como figuran en el mail, **sin convertir ni estimar**.

Incluye: comprobantes de Canva, EBANX, Topaz, OpenAI, Anthropic, hosting, dominios.

**Escala a Nicolás** si: hay un cobro fallido, un aviso de vencimiento de tarjeta, una
suba de precio, o un monto que supera los USD 200 en un solo cargo.

## D. Consultas de la web

**Esta es la categoría más importante de la casilla.** Son leads.

Identificación: remitente `onboarding@resend.dev` (o cualquier remitente de Resend) con
asunto `CONTACTO_WEB`. El cuerpo trae siempre tres campos parseables: Nombre, Mail,
Consulta.

Acción:
1. Extraer nombre, mail y texto de la consulta.
2. Etiquetar el hilo con `NOMA WEB`.
3. Clasificar la consulta: ver la tabla de abajo.
4. Responder **al mail del contacto**, no a Resend. El remitente del mail es
   `onboarding@resend.dev`, así que responder al hilo no le llega al interesado nunca.
   Hay que componer un mail nuevo dirigido a la dirección que figura en el campo Mail.
5. Etiquetar `Claude/Respondido` y archivar.
6. Sumarlo al resumen diario con una línea por lead.

| Tipo de consulta | Acción |
|---|---|
| Interés comercial genuino con proyecto descripto | Responder con `plantillas/consulta-web.md`, pedir los datos que faltan, ofrecer llamada. Etiquetar `ACCION REQUERIDA` además, porque Nicolás tiene que agendar |
| Consulta vaga o de una sola palabra (ej: "legal", "hola") | Responder pidiendo que amplíe. No escalar todavía |
| Spam por el formulario, texto sin sentido, prueba | Archivar sin responder. No escalar |
| Pedido de trabajo o CV | Etiquetar `RRHH`, no responder, escalar en el resumen |
| Tema legal, reclamo o prensa | **No responder.** Etiquetar `ACCION REQUERIDA` y push inmediato |

Ojo con los duplicados: es común que la misma persona mande el formulario dos veces
seguidas. Respondé una sola vez.

### Control de conversaciones abandonadas

**Este chequeo es obligatorio en cada corrida.** El agujero más caro de esta casilla no es
el lead que nunca se contestó, es el que se contestó, respondió con la información que le
pedimos, y del lado del estudio nadie siguió. En la auditoría del 06/08/2026 aparecieron
tres casos, uno de ellos con 106 días de silencio.

Buscá los hilos donde el último mensaje es del interesado y no nuestro:

```
search_threads: label:NOMA WEB -in:draft
```

Para cada uno, mirá quién mandó el último mensaje. Si fue el interesado y pasaron más de
48 horas, etiquetá `ACCION REQUERIDA` y sumalo al resumen. Si pasaron más de 7 días,
además va push.

No redactes vos la respuesta a una conversación abandonada por semanas. Retomar un hilo
frío requiere decidir si se pide disculpas y si se ofrece algo concreto, y eso lo define
un humano.

## E. Interno del equipo

Cualquier remitente `@nomastudio.ai`.

**Nunca se archiva automáticamente y nunca se responde en nombre de Nicolás.**

Acción: dejar en inbox, etiquetar `ACCION REQUERIDA` si contiene una pregunta directa
dirigida a info@, e incluir en el resumen diario. Si es una conversación entre otros
miembros del equipo donde info@ está en copia, marcar leído y dejar sin etiqueta.

## F. Clientes

Mails de dominios de clientes activos (ver `02-remitentes.md`).

Acción: dejar en inbox, etiquetar con el label del cliente si existe, etiquetar
`ACCION REQUERIDA`, incluir en el resumen y mandar push si el mail contiene una
pregunta, un pedido con fecha o una queja.

**Nunca responder por tu cuenta un mail de cliente sobre alcance, precio, plazo o
entregable.** Podés responder un acuse de recibo si el mail lleva más de 24 horas sin
respuesta humana, usando `plantillas/derivacion.md`.

## G. Seguridad y códigos

Códigos de verificación, OTP, alertas de inicio de sesión, avisos de cambio de
contraseña.

Acción: **no tocar**. Dejar en inbox, sin leer, sin etiquetar. No mencionar el código en
ningún resumen ni notificación.

Si aparece un aviso de acceso no reconocido o de cambio de contraseña que nadie pidió:
push inmediato a Nicolás, con el nombre del servicio pero **sin el código**.

## H. Rebotes y errores de entrega

Remitente `mailer-daemon@googlemail.com` o similares.

Acción: etiquetar `Claude/Revisar`, archivar, y agrupar en el resumen. Importan porque
suelen indicar una dirección mal escrita en una lista interna o un alias que no existe.
Visto en la casilla: rebotes hacia `noma.claw@nomastudio.ai` y
`manuela.cortes@nomastudio.ai`.

## I. Solicitudes de acceso a archivos

Google Drive, Canva y similares pidiendo permiso sobre un documento.

Acción: **nunca otorgar acceso.** Etiquetar `ACCION REQUERIDA`, dejar en inbox, incluir
en el resumen indicando quién pide qué. Si el solicitante es de un dominio externo al
estudio, marcarlo explícitamente como externo en el resumen.

## J. Desconocido

Si un mail no encaja limpio en ninguna categoría: **no adivines**. Dejalo en inbox,
etiquetalo `Claude/Revisar` y sumalo al resumen en la sección de dudas. Al final de la
corrida, si hay tres o más mails en esta categoría, proponé en el registro del día una
regla nueva para cubrirlos.
