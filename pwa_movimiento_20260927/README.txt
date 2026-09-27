COSTO FUERA DE PANTALLA + MOVIMIENTO + PWA · agente.html · 2026-09-27
Desplegado: certeza-api-00057-t8w (commit 8051c8c). humo.sh: todo OK.
Pruebas: 351 en verde (343 previas + 8 en tests/test_agente_pwa_y_costo.py).
Motor, reglas, capital.py y calculos sin cambios.

1 COSTO
 Antes: "48 de 48 fotos descritas / Costo del analisis: US$0.0xx."
 Ahora: "48 de 48 fotos analizadas" (y "Quedan N fotos sin analizar" si aplica).
 El costo sigue registrado en la bitacora (evento analisis_visual, campo
 costo_usd) y lo imprime infra/analizar_fotos.py. La API no cambio: solo
 la pantalla dejo de mostrarlo. Una prueba exige que el JS no mencione
 costo_usd ni US$.

2 MOVIMIENTO (medido en Chromium, estilos calculados)
 Esqueleto: brillo 1.4 s gris 10 % -> 1.2 s con tono del agente, pico 22 %.
 Tarjetas al abrir una auditoria ya hecha: antes todas en el mismo cuadro;
   ahora materializar 420 ms con retraso 0, 50, 100... tope 400 ms
   (tarjeta 3 -> +150 ms, tarjeta 20 -> +400 ms).
 Filas nuevas de documentos/fotos: fundido + 10 px, 280 ms, escalonadas.
   Solo las que no se habian pintado: el buscador y la cola repintan la
   lista y animar todo de nuevo cansaria.
 Tras decidir un hallazgo las tarjetas NO se vuelven a escalonar.
 Boton Auditar: al tocar baja 1 px, encoge a 97 %, se ilumina y muestra
   un anillo; sombra azul en reposo. Transiciones 120/200 ms.
 prefers-reduced-motion: tarjetas y filas con fundido simple sin retraso;
   el brillo del esqueleto se cambia por latido.
 Montos: sin cambios, siguen sin animarse.

3 PWA
 manifest.json (standalone, inicio /app), sw.js, iconos 192/512/180.
 sw.js NO guarda nada en cache: una auditoria vieja mostrada sin red se
 leeria como vigente. Solo muestra "Sin conexion" si la pagina no carga
 (probado con el servidor detenido: 503 + aviso).
 Rutas en certeza/api.py con lista cerrada de 6 archivos; unico cambio de
 servidor. sw.js va sin cache HTTP para que un cambio llegue al recargar.
 Produccion (Chrome, telefono 390 px): Page.getInstallabilityErrors = [],
 manifest sin errores, service worker activo, 0 errores de CSP o JS.
 ICONO PROVISIONAL (icono_provisional_512.png): el mismo "CZ" del avatar.
 Pendiente I: decision de Abner sobre color/identidad del logo.

QUE NO SE HIZO
 Capturas antes/despues de la animacion: no hay arnes de datos sinteticos
 guardado de la sesion anterior; se midio con estilos calculados.
