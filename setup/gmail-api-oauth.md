# Desbloquear el envío de mails

## El problema

El conector de Gmail que usa el agente puede leer, buscar, etiquetar, archivar y **crear
borradores**, pero no puede **enviar**. Tampoco puede crear filtros. Son limitaciones del
conector, no de la cuenta.

Mientras esto no se resuelva, el agente deja las respuestas como borrador y avisa. Eso
funciona, pero requiere que un humano entre a Gmail y apriete Enviar en cada una.

## Las tres salidas posibles

### Opción A: dejarlo en borradores

Cero trabajo de setup. El agente redacta, vos revisás y mandás. Para 17 consultas
acumuladas y una o dos por día, son unos minutos diarios.

Es la opción más segura para las primeras semanas: te deja ver exactamente qué habría
mandado el agente antes de darle la llave.

### Opción B: Resend, la vía que ya usa la web

El formulario de nomastudio.ai ya manda mails con Resend. Si el estudio tiene una API key
de Resend, el agente puede mandar mails con una llamada HTTP simple, sin OAuth ni consola
de Google.

Requisito: el dominio `nomastudio.ai` tiene que estar **verificado en Resend**. Hoy los
mails del formulario salen desde `onboarding@resend.dev`, que es la dirección de prueba de
Resend, lo cual sugiere que el dominio todavía no está verificado. Verificarlo es agregar
tres registros DNS, unos 15 minutos.

Ventaja: simple, y de paso arregla que los mails del formulario salgan desde una dirección
del estudio en vez de una genérica de Resend, que mejora bastante la entregabilidad.

Desventaja: los mails enviados no quedan en la carpeta Enviados de Gmail, quedan en el log
de Resend. Se puede arreglar mandando copia oculta a info@.

### Opción C: OAuth de la API de Gmail

Es la solución completa: permite enviar desde la casilla real, y además crear los filtros
por API en vez de a mano.

Pasos, una sola vez:

1. Entrar a Google Cloud Console con la cuenta que administra nomastudio.ai.
2. Crear un proyecto nuevo, por ejemplo `info-gmail-agent`.
3. Habilitar la **Gmail API**.
4. En Credenciales, crear un **ID de cliente de OAuth** de tipo Aplicación de escritorio.
5. En la pantalla de consentimiento, agregar los scopes:
   - `https://www.googleapis.com/auth/gmail.send`
   - `https://www.googleapis.com/auth/gmail.settings.basic`
6. Autorizar una vez con la cuenta `info@nomastudio.ai` y guardar el **refresh token**.
7. Cargar `GMAIL_CLIENT_ID`, `GMAIL_CLIENT_SECRET` y `GMAIL_REFRESH_TOKEN` como variables
   de entorno del entorno de ejecución del agente.

Como el dominio es de Google Workspace, un administrador también puede resolverlo con una
cuenta de servicio con delegación de dominio, que evita el refresh token. Es más prolijo
si a futuro se van a gestionar varias casillas.

**Aviso de seguridad:** cualquiera de estas credenciales le da a un proceso automático la
capacidad de mandar mails en nombre del estudio. Nunca las commitees a este repositorio.
Van como variables de entorno y nada más.

## Recomendación

Arrancar con la **opción A** durante dos semanas. Es tiempo suficiente para leer los
borradores que genera el agente y confirmar que el tono y el criterio son los correctos.
Si a las dos semanas no hubo que corregir ningún borrador, pasar a la **opción B**, que es
la de menor fricción y de paso mejora el formulario de la web.
