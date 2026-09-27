FOTOS VISIBLES + MOVIMIENTO HONESTO EN agente.html · 2026-09-27
Desplegado: certeza-api-00055-cv6 (commits bf795c3 y 4116b0e). humo.sh: todo OK.
Pruebas: 343 en verde (336 previas + 1 de CSP + 6 de movimiento). Motor,
reglas, capital.py y calculos sin cambios. Capturas: solo obras sinteticas
(este repo es publico); las de la foto real estan en
~/certeza-capturas-privadas/fotos_20260927/.

TAREA 1: FOTOS
 CSP: img-src 'self' data: blob: https://storage.googleapis.com. Es lo unico
   que cambio en el CSP; una prueba nueva exige esa lista exacta.
 CORS de la cubeta: NO se aplico. El permiso de la sesion bloqueo modificar
   gs://certeza-documentos-cinplag. Script listo (solo GET/HEAD, solo los dos
   origenes de certeza-api): bash ~/certeza/infra/cors_documentos.sh
   No bloquea las fotos: un <img> sin crossorigin no pide CORS. Solo haria
   falta si la app bajara la foto con fetch().
 Almacenamiento y firma sin cambios: misma URL firmada v4 de 10 minutos.
 PRUEBA con una foto real de Cabana Emma (PXL_20260916_142506303.MP.jpg):
   pagina desplegada en produccion (su HTML y su CSP reales) + URL firmada
   con el mismo codigo que la app. Los datos de la API salieron de un arnes
   local porque no hay sesion de Firebase para entrar.
   Telefono y computadora: miniatura y visor cargan, 200 image/jpeg desde
   storage.googleapis.com, 2160x3840 px, 0 violaciones de CSP, 0 errores JS.
   Falta: verlo con la sesion real de Adrian (pendiente G).
 COSTO que no cambio: cada miniatura baja la foto completa (~5 MB). La opcion
   (a) de pendiente F (miniatura reducida en el servidor) sigue siendo la
   recomendada para el telefono.

TAREA 2: QUE SE MOVIO
 1 Esqueleto con brillo al abrir una obra, al auditar y en cada foto que
   carga. Antes: pantalla en blanco (antes_*_1_abriendo_obra) o solo el
   boton "Auditando..." (antes_*_8_auditando).
 2 Progreso: POST /auditar es UNA peticion y el servidor no reporta etapas,
   asi que no hay progreso real que medir desde el frontend. Se usa esqueleto
   y una frase de lo que hace, sin porcentaje. La barra de la consola, que
   antes se llenaba paso a paso DESPUES de que el servidor ya habia
   respondido, ahora va llena y dice "auditoria terminada, repaso de N
   pasos". En el analisis de fotos si hay avance real (procesadas/total del
   servidor): ahi se agrego una barra.
 3 Botones se hunden al tocarlos (tambien pestanas, obras, visor, miniatura).
   Palomita que se dibuja cuando el servidor confirma: decision guardada,
   nota, autorizacion registrada, informe o exportacion descargados. Las
   cifras finales entran con fundido de 280 ms.
 4 Visor y cambio de pestana u obra con fundido; la foto entra con fundido.
 5 prefers-reduced-motion: antes se apagaba TODO (las cosas saltaban). Ahora
   desplazamientos, escalas y barridos se cambian por fundidos; la hoja se
   funde en vez de deslizarse. Ver despues_telefono_reducido_*.
 6 Transiciones entre 80 y 500 ms (tokens 120/200/280/420). Medido en
   Chromium: antes llegaba a 620 ms; despues 200 a 450 ms. Los ciclos
   continuos (brillo del esqueleto 1.4 s, luz del agente) no son
   transiciones: indican trabajo en curso.

QUE SE DEJO QUIETO A PROPOSITO
 - Ningun monto se anima. Nada cuenta hacia arriba; la cifra se asigna tal
   cual llega del servidor y solo aparece con fundido.
 - Textos con montos ya no se teclean letra por letra en la consola: un
   "$150,000" que va apareciendo digito a digito parece calculado en vivo.
   Se muestran enteros con fundido.
 - Sin porcentaje de avance en la auditoria (no hay dato real).
 - Sin palomita en "Descargar" foto: abre la URL y la app no puede saber si
   la descarga termino.
 - tests/test_agente_movimiento.py vigila todo lo anterior.

COMO SE PROBO
 API local en memoria con dos obras sinteticas, demoras artificiales (2.5 s
 al auditar, 1.6 s al leer la auditoria, 1.4 s por foto) para poder capturar
 los estados de carga. Chromium sin interfaz, telefono 390 px y computadora
 1280 px, version anterior (commit 9336358) y nueva. 0 errores de JS.
 Una corrida de la suite tuvo 1 falla que no se repitio en 9 corridas mas;
 no se pudo identificar cual fue.
