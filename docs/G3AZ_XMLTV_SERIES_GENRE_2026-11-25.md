# G3AZ — Corrección de géneros XMLTV para series

Fecha: 2026-11-25

## Objetivo

Evitar que las categorías secundarias de programas marcados como
`Series` generen códigos DVB propios de películas.

Ejemplos anteriores al parche:

- Series + Comedia: 0x14.
- Series + Policiacas: 0x11.
- Series + Animación: 0x55.
- Series + Telenovela: 0x15.

La solución conserva las categorías textuales del XMLTV y evita
asignar esos códigos DVB incorrectos.

## Implementación

Paquete: `tvheadend43`.

Fuente base:
`a4a1ce5ee5e6f6448f2386e24f370b97143fce8e`

Parche nuevo:
`tvheadend43-11-g3az-series-genre.patch`

SHA256:
`9a0916dd40ab8a656375d770a44acefae25118c1c3fb17238a6a99e48cfae890`

Dependencia: parche 09 de géneros españoles.

Se conserva independiente el parche 10 de mantenimiento IPTV.

Revisión anterior: PKG_REV=6.
Nueva revisión: PKG_REV=7.

Addon generado: `service.tvheadend43-21.2.7.zip`.

## Resultados del laboratorio

- P81: importación de géneros XMLTV reales.
- P86: reproducción de clasificaciones incorrectas.
- P90/P91: desarrollo de la corrección aislada.
- P93B: compilación nativa x86_64 experimental PASS.
- P96S: series sin códigos DVB y categorías conservadas.
- P96S-B: corrección de un evento existente sin cambiar eventId.
- P97: comprobación de categorías de películas sin la marca Series.
- P102: cuatro series importadas y verificadas por HTSP v44, PASS 4/4.
- P104: aplicación de parches ARM32 y AArch64, PASS.
- P105T: integridad y aplicación del parche, PASS.
- P108: identificación del mecanismo de aplicación de parches.
- P111: respaldo anterior verificado.
- P112: compilación oficial CoreELEC Amlogic-ng.arm, PASS.
- P113: auditoría ARM32, ZIP 21.2.7 y respaldo.
- P114: diferencia de tamaño por eliminación de símbolos.
- P115: reproducción exacta del binario del ZIP mediante
  `armv8a-libreelec-linux-gnueabihf-strip --strip-unneeded`, PASS.

## Artefactos locales

Respaldo anterior:
`/home/ubuntu/g3az-p111-backup`

Registro de compilación:
`/home/ubuntu/g3az-p112-build.log`

Nuevo ZIP:
`build.CoreELEC-Amlogic-ng.arm-21/addons/tvheadend43:target/service.tvheadend43-21.2.7.zip`

SHA256 del ZIP:
`a4c6d8b5bcaa1b62502c900fa5603d7d6ab77383708656d7ed15abd2c3079b2f`

SHA256 del binario del ZIP:
`2c47d2b41eefbcb638ff2defa5f78822f0f825921445fa6d444cb0e0efb41711`

## Límites de validación

La compilación ARM32 y el empaquetado están verificados.

No se ha demostrado todavía en el nuevo binario ARM32:
- La presentación visual definitiva de géneros en Kodi.
- La regresión funcional completa del mantenimiento IPTV/CUID.
- La instalación y el funcionamiento en el MASTER CoreELEC.

No publicar a producción hasta completar esas comprobaciones.

## Política de desarrollo

- Preservar el checkpoint estable `51ff9b471c`.
- No alterar los parches anteriores.
- Mantener pruebas aisladas antes de desplegar.
- Conservar el respaldo anterior y los SHA256.
- No modificar directamente bases de datos PVR de Kodi.
- Publicación del addon a producción a cargo del mantenedor.
