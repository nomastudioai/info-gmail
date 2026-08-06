# Procedimiento de la corrida diaria

Se ejecuta una vez por día, de lunes a viernes. Duración estimada: 5 a 15 minutos.

Antes de arrancar: leé `CLAUDE.md` y los tres archivos de `reglas/`.

---

## IDs de etiquetas

Los IDs no son los nombres. Estos son los que hay que pasarle a `label_thread`:

| Etiqueta | ID |
|---|---|
| ACCION REQUERIDA | `Label_1` |
| Claude/Respondido | `Label_3` |
| Claude/Revisar | `Label_4` |
| Alertas Sistema | `Label_5` |
| NOMA WEB | `Label_3701034837447704634` |
| Pagos y Suscripciones | `Label_5502641690105706794` |
| RRHH | `Label_8611562000357290868` |
| Google ADS | `Label_3275994120020927317` |
| Metricool | `Label_4066859991388172476` |
| META | `Label_4829777646964733173` |
| Canva | `Label_2420186015805118946` |
| Calendar | `Label_5817457758401035416` |
| LinkTree | `Label_5414366062699422034` |

Si creás una etiqueta nueva, agregala a esta tabla en el mismo commit.

Recordá: **archivar es `unlabel_thread` con `INBOX`**. **Marcar leído es `unlabel_thread`
con `UNREAD`**. Los dos se pueden mandar juntos en la misma llamada.

---

## Paso 1: leer el registro anterior

Abrí el archivo más reciente de `registro/`. Sirve para dos cosas: no volver a procesar lo
que ya procesaste, y ver si quedó algo pendiente de ayer que hoy ya tiene respuesta.

## Paso 2: traer los mails nuevos

```
search_threads: in:inbox newer_than:2d
```

Se usan 2 días, no 1, para cubrir el hueco si una corrida falló. Los que ya tengan
`Claude/Respondido`, `Claude/Revisar` o `Alertas Sistema` ya fueron procesados: saltealos.

Si vienen más de 50, paginá con `pageToken` hasta terminar. No proceses solo la primera
página y declares el día terminado.

## Paso 3: clasificar y actuar

Para cada hilo, aplicá `reglas/01-clasificacion.md`. El orden importa, porque un mail puede
encajar en más de una categoría. Evaluá en este orden y frená en la primera que dé:

1. ¿Es código de verificación o seguridad? No tocar. Siguiente.
2. ¿Es del equipo interno `@nomastudio.ai`? Categoría E.
3. ¿Es de un dominio de cliente? Categoría F.
4. ¿Es una consulta web `CONTACTO_WEB`? Categoría D.
5. ¿Es una alerta de sistema? Categoría B, mirando la excepción de incidente.
6. ¿Es factura o pago? Categoría C.
7. ¿Está en la lista de ruido? Categoría A.
8. ¿Es un rebote? Categoría H.
9. ¿Es pedido de acceso a un archivo? Categoría I.
10. Ninguna: categoría J, etiquetar `Claude/Revisar` y dejar en inbox.

Para las consultas web, leé el cuerpo completo con `get_thread`, no te fíes del snippet.
El campo Consulta suele estar cortado en el snippet.

## Paso 4: responder

Solo para consultas web que califiquen según la categoría D.

Mientras el envío esté bloqueado (ver `setup/gmail-api-oauth.md`), usá `create_draft`:

- `to`: la dirección del campo Mail del cuerpo, **no** `onboarding@resend.dev`
- `subject`: `Re: tu consulta a NoMa Studio AI`
- Sin `replyToMessageId`, porque es un mail nuevo, no una respuesta al hilo de Resend
- Cuerpo: la plantilla que corresponda de `plantillas/consulta-web.md`

Cuando el envío esté desbloqueado, se manda directo y se etiqueta `Claude/Respondido`.

**Antes de mandar cualquier respuesta, releela contra las reglas duras de `CLAUDE.md`.**
Si contiene un precio, un plazo o un compromiso, no sale: va a borrador con
`ACCION REQUERIDA`.

## Paso 5: escalar

Aplicá `reglas/03-escalamiento.md`. Máximo 3 push por día.

Para derivar un mail a Nicolás, usá `create_draft` con la plantilla A de
`plantillas/derivacion.md` dirigido a `nicolas@nomastudio.ai`.

## Paso 6: escribir el registro

Creá `registro/AAAA-MM-DD.md` con esta estructura:

```markdown
# Corrida del DD/MM/AAAA

## Números
- Mails procesados: N
- Archivados como ruido: N
- Alertas de sistema agrupadas: N
- Facturas registradas: N
- Consultas web: N
- Escalados a Nicolás: N
- Sin clasificar: N

## Requiere tu atención
(una línea por ítem, con el motivo y el link al hilo. Si no hay nada, escribí "Nada.")

## Consultas web del día
(nombre, mail, resumen de la consulta en una línea, qué se respondió)

## Facturas
(servicio, monto con moneda, fecha)

## Dudas
(mails que no supiste clasificar y por qué)

## Cambios propuestos a las reglas
(si detectaste un patrón nuevo que convendría automatizar)
```

## Paso 7: actualizar las reglas

Si apareció un remitente nuevo que clasificaste, agregalo a `reglas/02-remitentes.md`.
Este paso es el que hace que el sistema mejore solo. No lo saltees.

## Paso 8: commit

```
git add -A && git commit -m "registro: corrida del DD/MM/AAAA"
git push -u origin claude/nomastudio-email-management-6dyyu5
```

## Paso 9: avisar

Un solo push al celular con el titular del día. Si no pasó nada que requiera atención, el
push igual se manda pero en una línea: `Casilla al día. N mails procesados, nada urgente.`

Si hay algo urgente, ese push va primero, en el momento en que lo detectaste, no al final.

---

## Si algo falla

- **El conector de Gmail no responde:** no reintentes en loop. Anotá el error en el
  registro del día y mandá un push avisando que la corrida falló.
- **Un mail no se deja clasificar:** `Claude/Revisar` y seguir. Nunca frenes la corrida
  entera por un mail.
- **Dudás si archivar algo:** no lo archives. El costo de un mail de más en el inbox es
  cero. El costo de enterrar un lead es real.
