# Gestión de info@nomastudio.ai

Sistema de triaje automático de la casilla `info@nomastudio.ai` de NoMa Studio AI.

Una corrida diaria lee los mails nuevos, archiva el ruido, etiqueta lo que importa,
responde las consultas que entran por el formulario de la web y avisa por notificación al
celular cuando hace falta una decisión humana.

## Cómo está armado

```
CLAUDE.md              Identidad, reglas duras y capacidades. Lo primero que se lee
PROCEDIMIENTO.md       Los 9 pasos de la corrida diaria, con los IDs de etiquetas
reglas/
  01-clasificacion.md  Las 10 categorías de mail y qué se hace con cada una
  02-remitentes.md     Directorio de quién es quién. Crece con cada corrida
  03-escalamiento.md   Cuándo interrumpir a un humano y cuándo no
plantillas/
  consulta-web.md      Respuestas a leads, en español, inglés y portugués
  derivacion.md        Derivación interna y acuses de recibo
setup/
  filtros-gmail.md     Los 8 filtros nativos, explicados
  filtros-nomastudio.xml  Los mismos filtros, listos para importar
  gmail-api-oauth.md   Cómo desbloquear el envío automático
registro/              Un archivo por día con lo que pasó
```

## Por qué las reglas están en archivos y no en un prompt

Cada corrida diaria arranca en una sesión nueva, sin memoria de las anteriores. El
repositorio **es** la memoria. Un criterio que no esté escrito acá no existe al día
siguiente.

El corolario práctico: cuando el agente clasifica algo mal, no hay que retarlo, hay que
editar el archivo de reglas. La corrección queda para siempre.

## Las tres capas del sistema

1. **Filtros nativos de Gmail.** Gratis, instantáneos, corren sin agente. Se llevan el
   ruido de alto volumen. Es la capa de mayor retorno.
2. **Corrida diaria del agente.** Se ocupa de lo que requiere leer y entender: leads,
   mails de clientes, cosas ambiguas.
3. **Escalamiento a un humano.** Todo lo que compromete al estudio o que el agente no
   sabe resolver.

Cada capa existe para que la de arriba tenga menos trabajo.

## Estado

| Pieza | Estado |
|---|---|
| Reglas y procedimiento | Listo |
| Etiquetas en Gmail | Creadas |
| Corrida diaria automática | Activa, 08:35 ART de lunes a viernes |
| Filtros nativos | **Pendiente de importar el XML** |
| Envío automático de respuestas | **Bloqueado**, ver `setup/gmail-api-oauth.md` |
| Limpieza del backlog acumulado | Pendiente de aprobación |

## Operación manual

Para correr el triaje fuera de horario, pedirle a Claude: *corré el triaje de la casilla*.
Va a leer este repositorio y seguir `PROCEDIMIENTO.md`.
