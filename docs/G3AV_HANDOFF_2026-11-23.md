# G3AV — Continuidad EPG y Tvheadend KodiELEC

Fecha: 2026-11-23

## Arquitectura y equipos

Raspberry Pi 4 (192.168.1.100):
- Generación EPG mediante WebGrab++.
- Enriquecimiento, normalización horaria y publicación GitHub.

Ubuntu de desarrollo (192.168.1.114):
- Repositorio CoreELEC: /home/ubuntu/GitHub/KodiELEC
- Fuente Tvheadend: /home/ubuntu/tvheadend-g3an-upstream
- Toolchain ARM32:
  build.CoreELEC-Amlogic-ng.arm-21/toolchain/bin/
  armv8a-libreelec-linux-gnueabihf-gcc

Master KodiELEC:
- Addon: service.tvheadend43
- Versión instalada: 21.2.4
- Ejecutable: ELF32, máquina ARM (40)
- Nueva versión objetivo: 21.2.5

## EPG — Producción completada

Repositorio:
https://github.com/masQelec/epg

URL RAW:
https://raw.githubusercontent.com/masQelec/epg/main/guide.xml

Publicador:
  /root/.wg++/update_git.py

Módulos:
  /root/.wg++/epg_genre_engine.py
  /root/.wg++/epg_timezone_stream.py
  /root/.wg++/genre_catalog.json

Fuente validada:
  /root/g3av-epg-lab/g3av-release/

Respaldo reversible:
  /root/g3av-epg-lab/g3av-gq-backup-zx80jwf3

Cron existente (no modificado):
- 06:00: /root/.wg++/run.sh
- 00:00: /usr/bin/python3 /root/.wg++/update_git.py

G3AV-GQ_RESULT=PASS
G3AV-GR_RESULT=PASS
G3AV_GR_RAW_INTEGRITY=PASS

Publicación de referencia:
- 35.373 programas
- 240 canales
- 16.066 correcciones de zona horaria
- 1.186 géneros enriquecidos
- XMLTV válido

Commit GitHub:
fbc7b16130e9b6af08416d5d57f6826b9ba43ea8

SHA256 de esa publicación:
fae873170839dbbe4eaad653e8bc797918e509ec25d4ccc70587c94dd974f689

Se descargó el RAW y se comprobó identidad byte por byte.
Los SHA pueden cambiar en publicaciones posteriores.

## Compilación Tvheadend ARM32

Receta:
packages/addons/service/tvheadend43/package.mk

Fuente base:
a4a1ce5ee5e6f6448f2386e24f370b97143fce8e

PKG_VERSION_NUMBER:
4.3-g3au-a4a1ce5e

Revisión anterior: 4
Revisión objetivo: 5

Comando de compilación anteriormente utilizado:
PROJECT=Amlogic-ce ARCH=arm DEVICE=Amlogic-ng scripts/create_addon tvheadend43

ZIP anterior verificado:
build.CoreELEC-Amlogic-ng.arm-21/addons/tvheadend43:target/service.tvheadend43-21.2.4.zip

## Parches CoreELEC

07 — IPTV/CUID:
tvheadend43-07-g3au-iptv-cuid.patch

SHA256:
78e2abec4dfeda3c871557364155549239c1caba970a80fbf7b17960465247ee

Coincide exactamente con los cambios actuales IPTV.

08 — Compatibilidad FFmpeg 6:
tvheadend43-08-ffmpeg6-profile-compat.patch

09 — Géneros XMLTV españoles:
tvheadend43-09-g3av-xmltv-spanish-genres.patch

G3AV-GX6:
- Parche 09 aplicable sobre la fuente CoreELEC.
- Equivalencia con modificación de desarrollo PASS.
- Semántica ETSI original preservada.
- Parche instalado PASS.

Diferencia conocida entre fuentes:

CoreELEC:
  epg_genre_list_add_by_eit(egl, cat_val);
  cat_flag = 1;

Desarrollo:
  cat_flag = epg_genre_list_add_by_eit(egl, cat_val);

Se conserva la variante CoreELEC para no introducir
cambios adicionales con códigos ETSI duplicados.

## Pruebas EPG superadas

Laboratorio Tvheadend G3AV-GD:
- La Promesa: Telenovela, 0x15
- La que se avecina: Comedia, 0x14
- NCIS: Policiacas, 0x11
- Los Simpson: Animación, 0x55
- Independence Day: Ciencia ficción, 0x13

Pendiente validación visual en Kodi Master.

## Método de trabajo

- Operaciones por etapas y resultados PASS/FAIL.
- Ejecutar scripts dentro de subshell, sin cerrar SSH.
- Preservar modificaciones existentes sin sobrescribirlas.
- Mantener identidad IPTV mediante CUID.
- No alterar manualmente la base de datos Kodi.
- No utilizar binarios Ubuntu/aarch64 en Master ARM32.
- Respaldar el addon Master antes de instalar.
- Verificar ZIP, versión, ELF y SHA256 antes del despliegue.
- Mantener independientes la publicación EPG y la compilación.

## Próximos pasos

1. Completar G3AV-HA y verificar PKG_REV=5.
2. Determinar reconstrucción limpia de tvheadend43.
3. Compilar addon CoreELEC ARM32 con parches 07/08/09.
4. Verificar versión 21.2.5 y binario ELF32 ARM.
5. Crear ZIP y comprobar SHA256.
6. Respaldar e instalar manualmente en Master.
7. Validar estabilidad IPTV/CUID y géneros en Kodi.
8. Documentar las pruebas y confirmar cambios en Git.

El estado de compilación e instalación es pendiente.

## G3AV-IH — Compilacion 21.2.6

CoreELEC ARM32, service.tvheadend43.

PKG_REV=6.

G3AV-IE:
- Cinco parches inventariados.
- Aplicación dry-run PASS.
- Parche IPTV idéntico al diff de desarrollo.

G3AV-IF:
- Compilación completada correctamente.
- ALL ADDONS BUILT SUCCESSFULLY.

El validador inicial buscaba el ZIP 21.2.5 en
el directorio de addons, pero estaba respaldado en:

/home/ubuntu/g3av-tvheadend-release/service.tvheadend43-21.2.5-baseline.zip

Nuevo ZIP:
build.CoreELEC-Amlogic-ng.arm-21/addons/tvheadend43:target/service.tvheadend43-21.2.6.zip

Informe de verificación:
 /home/ubuntu/g3av-tvheadend-release/g3av-ih-verification.json

Estado:
- Compilación completada.
- Pendiente validación funcional de la candidata.
- Master sigue utilizando 21.2.5.
- No se ha demostrado aún la necesidad individual de cada parche.
