INSTALAR EN UN TOQUE Y ESTADO VACIO DE OBRA · 2026-09-27
Desplegado: certeza-api-00063-4qp (commit 33bf266). humo.sh: todo OK.
Pruebas: 371 en verde (358 previas + 13 en tests/test_agente_instalar_y_vacio.py).
Solo agente.html: motor, reglas y calculos sin cambios.

1 INSTALAR CERTEZA (instalar_telefono / instalar_computadora)
 El aviso del navegador (beforeinstallprompt) se guarda y no sale solo.
 La tarjeta con el icono "CZ." aparece ~2 s despues de que termina un
 informe con hallazgos; nunca al abrir la app.
 Computadora/Android (Chrome, Edge): "Instalar" dispara el aviso guardado.
 iPhone/iPad (Safari no tiene el evento): "Compartir -> Agregar a inicio"
   con la guia dibujada; boton "Entendido".
 Otros navegadores sin el evento: no se ofrece nada (no habria boton que
   funcione).
 Instalada, rechazada o "Entendido": no se vuelve a ofrecer en ese equipo.

2 ESTADO VACIO (vacio_auditoria_* / vacio_documentos_*)
 Formatos verificados contra los adaptadores, en dos grupos:
   Lo lee y lo revisa: PDF con texto, Excel, CSV, BC3, fotos JPG/PNG/HEIC.
   Lo guarda en el expediente: Word, DWG, DXF, IFC.
 Word y planos pasan el antivirus y se guardan, pero ningun adaptador los
 lee hoy: ponerlos como "leidos" habria sido prometer de mas. Una prueba
 falla si la lista se sale de lo que el servidor lee de verdad.
 Tambien se corrigio el texto de la zona de subida, que decia "imagen de
 factura" como si se leyera.

3 CONFIRMACIONES (reconocido_subida_* / reconocido_conceptos_*)
 Al quedar listo: "Excel reconocido: listo para leer el presupuesto" (el
 formato lo decide el servidor por contenido, no por extension).
 Tras auditar: "Presupuesto reconocido: 88 conceptos leidos", tambien fijo
 bajo la fila del documento. La cifra es conceptos_leidos del servidor.
 El servidor no lee el presupuesto al subirlo, asi que antes de auditar no
 hay cifra y no se inventa (decision J en pendiente.txt).

Capturas: servidor local en memoria, Chromium a 390 px (UA iPhone) y
1280 px. Los datos de las filas (88 conceptos, archivos) son simulados en
el navegador; el camino del servidor lo cubren las pruebas. Sin scroll
horizontal ni errores de JS en ninguno de los dos.
