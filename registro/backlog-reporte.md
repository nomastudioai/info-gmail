# Reporte de backlog, previo a la limpieza

Estado al 06/08/2026. **Nada de esto se ejecutó todavía.** Es la propuesta para aprobar.

## Situación

| Métrica | Valor |
|---|---|
| Conversaciones en inbox | 3.030 |
| Sin leer | 2.403 |
| Antigüedad | Desde abril de 2026 en adelante |

El estimador de Gmail se topea en 201 resultados por consulta, así que los totales por
remitente que van abajo son cotas mínimas, no números exactos. El conteo real se informa
al ejecutar.

## Lote 1: ruido puro. Archivar y marcar leído

Riesgo: nulo. Todo se recupera buscando `in:anywhere`.

| Remitente | Tipo |
|---|---|
| `noreply-dmarc-support@google.com` | Reporte DMARC diario |
| `noreply@dmarc.yahoo.com` | Reporte DMARC diario |
| `dmarcreport@microsoft.com` | Reporte DMARC diario |
| `no-reply@mail.nordvpn.com` | Promoción |
| `news@topazlabs.com` | Newsletter |
| `em@em1.cloudflare.com` | Newsletter |
| `welcome@t.brevo.com` | Promoción |
| `noreply@skool.com` | Notificación social |
| `noreply@microsoftadvertising.com` | Promoción |
| `no-reply@artists.spotify.com` | Notificación social |
| `glauber@turso.tech` | Newsletter |
| `bingwb@microsoft.com` | Onboarding de producto |

Los tres remitentes de DMARC solos son 3 mails por día. Desde abril son varios cientos.

## Lote 2: alertas de sistema. Etiquetar y archivar

Riesgo: bajo. Se conservan etiquetadas por si hace falta auditar un incidente viejo.

`hello@notify.railway.app`, `noreply@supabase.com`, `sa@ahrefs.com`, `sc-noreply@google.com`

**Excepción:** los mails de Railway cuyo asunto contenga `crashed` o `failed` quedan en
inbox. Son incidentes, no rutina.

## Lote 3: facturación. Etiquetar y archivar

Riesgo: bajo. Quedan agrupadas bajo `Pagos y Suscripciones`, que es donde conviene
tenerlas para cuando haya que buscar un comprobante.

`payments@ebanx.com`, `no-reply@account.canva.com`, `no-reply@topazlabs.com`, y cualquier
asunto con factura, invoice, receipt, comprobante o payment.

## Lote 4: basura del formulario web. Archivar

Riesgo: bajo, pero conviene que las mires antes porque son las únicas que involucran
personas reales.

| Fecha | Nombre | Consulta textual |
|---|---|---|
| 03/08/2026 | Lorena Moura | "legal", y en un segundo envío "Estou passando bem" |
| 08/07/2026 | Nelly Wackes | "I need to create a Poster for university" |
| 14/06/2026 | Enhel | "Necesito usar esta ia" |
| 14/06/2026 | Ñessi | "Poner en cuatro a fotos" |
| 31/05/2026 | Pablo Castro | "Me gusta Videos", y en un segundo envío una cadena de números |
| 30/05/2026 | Hector Gonzalez | "Me gustaría pasarla bien" |
| 29/05/2026 | Dyland Tavara | "En crear vídeos", enviado dos veces |
| 09/05/2026 | maria | Texto obsceno |

Ninguna tiene contenido comercial. Propongo archivar las ocho sin responder.

## Lo que NO se toca

- Todo mail de `@nomastudio.ai`
- Todo mail de dominios de clientes
- Códigos de verificación, OTP y alertas de seguridad
- Las conversaciones de `CONTACTO_WEB` con contenido real, respondidas o no
- Cualquier hilo con `ACCION REQUERIDA`
- Cualquier hilo donde info@ haya enviado una respuesta

## Cómo se ejecuta

Por lotes, empezando por el 1, que es el de riesgo cero. Después de cada lote se informa
el número real de conversaciones archivadas. Si algo se archivó por error, se recupera
buscando por remitente y volviendo a poner el label `INBOX`.

Ninguna operación de este plan borra nada ni manda nada a la papelera.
