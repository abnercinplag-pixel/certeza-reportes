REDISENO DE agente.html · 2026-09-26 · antes y despues por punto
Desplegado: certeza-api-00053-2x5 (commit 3b25500). humo.sh: todo OK.
Pruebas: 336 en verde (319 previas intactas + 13 de endpoints + 4 de la
pantalla). Motor, reglas y capital.py sin cambios.
Capturas aqui: solo obras sinteticas (este repo es publico). Las de la obra
real quedan en ~/certeza-capturas-privadas/rediseno_20260926/ (36 archivos,
telefono 390px y computadora 1280px, antes y despues).

Como se probo: API local en memoria con la obra real de Adrian copiada de
Firestore (ultimo corte, 17 hallazgos, decisiones, 70 documentos con
metadatos de fotos) + 3 obras sinteticas (con corte, limpia, sin auditar).
Recorrido con Chromium sin interfaz. Cero errores de JavaScript.
La cifra que muestra la obra real es la suma de 7 puntos con monto,
verificada a mano contra los montos guardados en Firestore.

REGLA B (cero aritmetica en JS)
 antes: el JS sumaba montos, restaba lo retenido del pago, animaba la cifra
        contando y redondeaba a pesos enteros. 17 lineas lo hacian.
 despues: 0. Totales en GET /api/obras/{id}/resumen (Decimal, ya
        formateados). tests/test_agente_sin_aritmetica.py falla con el
        archivo anterior (17 lineas marcadas) y pasa con el nuevo.

1. ANTES DE AUDITAR
 antes: boton "Auditar" pequeno junto al selector.
 despues: boton grande protagonista con "Revisa el presupuesto y las fotos
        de esta obra". Ver *_1_sin_auditar.png.
2. DESPUES DE AUDITAR
 antes: "monto acumulado en hallazgos senalados", en rojo, redondeado, con
        una barra de "confianza 60%" que no salia de ningun dato.
 despues: sin corte -> "Diferencias detectadas en el presupuesto: $X",
        "Es lo presupuestado: no es dinero pagado ni capital en riesgo" y
        "Captura el corte de obra para calcular el capital en riesgo".
        Con corte -> "Si autorizas el pago completo, $X quedarian sin obra
        que los respalde". Color neutro. La barra de confianza inventada se
        quito. "Auditar" pasa a "Volver a auditar", pequeno.
        El razonamiento de 20 pasos va plegado al volver a una obra.
3-5. HALLAZGOS EN DOS CAPAS
 antes: titulo tecnico del motor + tarjeta que se desplegaba entera.
 despues: capa visible = "$monto: frase llana" (p. ej. "el mismo concepto
        aparece presupuestado con dos precios distintos"), categoria
        (Recomendacion: retener / Pide documento / Para tu conocimiento,
        decidida en el servidor), "que conviene pedir" y botones de
        decision. "Ver detalle" plegado: texto del motor, evidencia con
        documento y fila, regla, estatus, revision humana, nota.
        Cierre solo en retencion: "Identificado antes de pagar. Recomendamos
        retener este monto hasta que se aclare."
6. FOTOS
 antes: la foto solo aparecia como texto en la evidencia.
 despues: miniatura junto a "lo que vio el analisis"; al tocarla, visor con
        zoom, descripcion, defectos, fecha (verificada o no, con motivo),
        ubicacion o su advertencia, Descargar y Ver original.
        LIMITE MEDIDO: hoy la imagen NO se puede mostrar. La CSP de la app
        solo admite img-src 'self' data: blob: y la cubeta de documentos no
        tiene CORS. El navegador bloquea la URL firmada (visto en consola).
        Como la orden prohibia tocar seguridad y almacenamiento, la
        miniatura dice "Toca para ver los datos de la foto" y el visor
        muestra los datos + Descargar. Se prueba una sola imagen por carga
        de pagina para no llenar la bitacora de descargas inutiles.
        Decision pendiente: ver pendiente.txt (F).
7-9. AUTORIZAR
 antes: barra fija verde "Autorizar" que marcaba TODOS los hallazgos como
        liberados, sin confirmar. Autorizas/Retienes calculados en JS.
 despues: panel "Resumen de tus decisiones" despues de los hallazgos:
        Autorizas / Retienes con origen ("$Y en N puntos sin resolver. Pago
        solicitado: $Z"). Boton sobrio "Autorizar el pago de $X" y "Dejar
        pendiente" del mismo tamano. Confirmacion con monto, lista de lo que
        queda sin resolver y "Esta decision se registra en la bitacora a tu
        nombre y con fecha"; Confirmar / Volver a revisar. Al terminar:
        "Autorizaste el pago de $X... registrado el <fecha> a nombre de
        <correo>". POST /pago-autorizado no toca decisiones; rechaza (409)
        si el monto o el corte cambiaron desde que se reviso.
        Obra de Adrian (sin corte): no hay boton; dice que no hay pago que
        autorizar. Correcto segun la orden.
10. OBRA LIMPIA
 antes: "$0" en verde y "No hay hallazgos que retengan el pago".
 despues: "Revisamos N conceptos y no encontramos diferencias con sustento,
        dentro de lo que se pudo revisar" + "Ver hasta donde se pudo
        revisar". Ver *_11_limpia.png.
EXTRA
 - Historial: ya no pinta $0 en cortes sin capital ("sin corte capturado");
   el cambio contra el corte anterior llega escrito del servidor.
 - Mensaje a la constructora: usa las cifras del servidor y "presupuestado".
 - El informe PDF no se toco.
