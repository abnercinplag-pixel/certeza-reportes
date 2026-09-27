MARCA FINAL DE CERTEZA · 2026-09-27
Desplegado: certeza-api-00059-5x6 (commit 2e15486). humo.sh: todo OK.
Pruebas: 358 en verde (351 previas + 7 en tests/test_marca.py).
Solo presentacion: motor, reglas, capital.py y calculos sin cambios.

ORIGEN DE LA MARCA
 No habia archivos de marca en este repo (solo logo_actual.txt, que es el
 avatar viejo). Se construyo desde la especificacion: Inter 600, azul
 #0B1F3A, verde #1F9E6B, punto verde como unico acento. Inter viene del
 paquete oficial rsms/inter v4.1 (OFL). Si hay un archivo oficial, se
 reemplazan los PNG de web/iconos/ con los mismos nombres y tamanos.

1 ENCABEZADO (encabezado_antes_* / encabezado_despues_*)
 Antes: avatar "CZ" con degradado azul-cian y "CERTEZA Auditor"; en
   telefono chico "CERTEZA" se encimaba con el selector de obras.
 Ahora: placa azul con "CERTEZA" en Inter 600 blanco y punto verde. A
   600 px o menos no cabe junto a obras y botones: queda el icono "CZ."
   (el mismo dibujo que el icono instalado). Al lado: "Auditor" + obra.
 El punto se dibuja como circulo (0.22 em): el glifo "." de Inter a 16 px
   mide ~2 px y el acento casi no se veia.
 entrar.html lleva el mismo wordmark (produccion_entrar_telefono.png).
 Sin animacion en la marca (antes el avatar tenia un aura pulsante).

2 TIPOGRAFIA
 Inter variable (352 KB, todos los pesos) y JetBrains Mono 400/500 en
 web/fuentes/, servidas por la app como woff2 (el formato comprimido del
 .ttf; el .ttf de Inter SemiBold queda en infra/fuentes/ para dibujar los
 iconos). Ya no se pide nada a fonts.googleapis.com ni fonts.gstatic.com.
 Titulos: Inter 600. Cifra protagonista: pasa de JetBrains Mono a Inter
 600 con cifras tabulares (no bailan entre obras).

3 ICONO PWA: "CZ." 512/192 en el manifest + 180 para iOS. A sangre en
 azul: sirve como icono normal y maskable (punto a 159 px del centro,
 zona segura 205 px). Produccion: 0 errores de instalabilidad.
 Quien ya instalo la app: Android actualiza el icono solo (puede tardar
 dias); en iPhone hay que quitarla y volver a agregarla.

4 FAVICON: "C." en favicon.ico (16/32/48) y PNG 32.

5 CSP: sin cambios. font-src ya incluia 'self'. Todavia admite
 fonts.googleapis.com/gstatic.com; ya no se usan y se podrian quitar
 (cambio de seguridad aparte, no se hizo).
