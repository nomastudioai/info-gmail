# Filtros nativos de Gmail

Estos filtros los ejecuta Gmail solo, gratis, instantáneo y sin que corra ningún agente.
**Es la pieza de mayor retorno de todo el sistema.** Cargar estos filtros baja el volumen
diario a la mitad y hace que el trabajo del agente sea liviano.

Hay dos formas de cargarlos: importar un archivo XML (rápido, recomendado) o crearlos a
mano uno por uno.

---

## Opción 1: importar el XML (5 minutos)

1. Descargá el archivo `filtros-nomastudio.xml` de esta carpeta.
2. En Gmail, andá a Configuración, pestaña **Filtros y direcciones bloqueadas**.
3. Abajo de todo, **Importar filtros**.
4. Seleccioná el archivo, revisá la lista y confirmá con **Crear filtros**.
5. Importante: **no** tildes "Aplicar también a las conversaciones que coinciden" en la
   primera pasada. Primero verificá durante unos días que los filtros no atrapen nada que
   no debían. Después sí se puede aplicar retroactivo.

## Opción 2: a mano

Configuración, Filtros y direcciones bloqueadas, Crear un filtro nuevo. Para cada fila de
la tabla, poné el criterio en el campo indicado y tildá las acciones.

| # | Campo | Criterio | Acciones |
|---|---|---|---|
| 1 | De | `noreply-dmarc-support@google.com OR noreply@dmarc.yahoo.com OR dmarcreport@microsoft.com` | Omitir recibidos, Marcar como leída, Aplicar etiqueta `Alertas Sistema` |
| 2 | Asunto | `Report Domain OR Report domain` | Omitir recibidos, Marcar como leída, Aplicar etiqueta `Alertas Sistema` |
| 3 | De | `no-reply@mail.nordvpn.com OR news@topazlabs.com OR em@em1.cloudflare.com OR welcome@t.brevo.com OR noreply@skool.com OR noreply@microsoftadvertising.com OR no-reply@artists.spotify.com OR glauber@turso.tech OR bingwb@microsoft.com` | Omitir recibidos, Marcar como leída |
| 4 | De | `sa@ahrefs.com OR sc-noreply@google.com` | Omitir recibidos, Aplicar etiqueta `Alertas Sistema` |
| 5 | De | `hello@notify.railway.app` **y** Asunto: `Security update` | Omitir recibidos, Marcar como leída, Aplicar etiqueta `Alertas Sistema` |
| 6 | De | `payments@ebanx.com OR no-reply@account.canva.com OR no-reply@topazlabs.com` | Omitir recibidos, Aplicar etiqueta `Pagos y Suscripciones` |
| 7 | Asunto | `CONTACTO_WEB` | Aplicar etiqueta `NOMA WEB`, Destacar, **Marcar siempre como importante**, No omitir recibidos |
| 8 | De | `@nomastudio.ai` | Marcar siempre como importante, No aplicar nunca el filtro de spam |

---

## Notas sobre el diseño de estos filtros

**El filtro 5 es deliberadamente angosto.** Solo atrapa los mails de Railway que dicen
`Security update`, que son puro ruido de mantenimiento. Los que dicen `crashed` o `failed`
siguen llegando al inbox, porque esos son incidentes reales. No lo amplíes a todo
`notify.railway.app` o vas a enterrar una caída de producción.

**El filtro 7 nunca archiva.** Las consultas de la web son lo único que genera ingresos en
esta casilla. Se destacan y se marcan importantes, jamás se sacan del inbox
automáticamente.

**El filtro 8 protege al equipo interno** de que un mail propio caiga en spam, cosa que
pasa con más frecuencia de la que uno espera cuando hay reenvíos y alias.

**Ninguno de estos filtros borra nada.** Todos usan "omitir recibidos", que es archivar.
Todo sigue siendo buscable con `in:anywhere`.

## Qué queda deliberadamente afuera

No hay filtro para el cold outreach comercial, porque los remitentes cambian todo el
tiempo y un filtro por palabra clave se lleva puestos mails legítimos. Eso lo sigue
resolviendo el agente, que puede leer el contexto.
