Crea un juego completo de simulación económica al estilo Victoria 3 en un ÚNICO archivo: /home/diegorl/juegos/victoria-sim/index.html

REQUISITOS Duros
- 100% autocontenido: sin dependencias externas, sin CDN, sin fetch, sin fuentes remotas. Debe funcionar offline al abrirlo con doble clic (file://).
- Todo el texto de la interfaz en español (claro y directo).
- Vanilla JS + CSS, sin frameworks. CSS moderno bienvenido (grid, custom properties).
- Cero errores en consola.
- Si de verdad no cabe bien en un archivo, permite index.html + game.js + style.css en la misma carpeta, pero prefiere un solo index.html.

ESTÉTICA (estilo Victoria 3)
- Fondo azul marino muy oscuro (#0d1520), acentos latón/oro (#c9a227), paneles tipo pergamino oscuro con bordes sutiles, cabeceras serif (Georgia), cuerpo sans-serif. Tooltips al hover. Sensación de mapa político del siglo XIX.

CONCEPTO
- Gobiernas la nación ficticia "Valoria" desde enero de 1836 hasta diciembre de 1936.
- Tick mensual con controles de velocidad (pausa, 1x, 2x, 4x).
- Victoria por puntuación al final (prestigio + PNB + nivel de vida medio). Derrota: revolución (inestabilidad al 100%) o bancarrota sostenida.

MECÁNICAS CENTRALES (estilo Victoria 3)
1) POBLACIÓN POR CLASES (pops): Terratenientes, Capitalistas, Artesanos, Obreros, Campesinos, Burócratas, Militares, Clero. Cada clase: población (miles), riqueza, renta media, % de poder político, satisfacción. La población crece según nivel de vida. Inmigración si el nivel de vida atrae, emigración si es bajo.
2) MERCADO DE MERCANCÍAS: grano, carne, madera, ropa, muebles, herramientas, hierro, carbón, petróleo, electricidad, papel, medicinas, artículos de lujo. Precio fluctúa cada mes por oferta vs demanda (clamp para que no explote). Mercado mundial simplificado: precio mundial con deriva aleatoria; importación/exportación automática si conviene según política arancelaria (libre comercio / mercantilismo / aislamiento).
3) EDIFICIOS: cada uno con coste (dinero + mercancías), tecnología requerida, trabajadores por clase, insumos, productos, nivel máximo, mantenimiento. Lista: granja de subsistencia, plantación de grano, hacienda ganadera, aserradero, mina de hierro, mina de carbón, pozo petrolero, fábrica textil, taller de muebles, fábrica de herramientas, acería, fábrica de papel, laboratorio farmacéutico, central eléctrica, plantación de lujo, puerto, banco (+capacidad de inversión), universidad (+investigación), hospital (+salud), arsenal (+poder militar). El jugador construye con tesoro público + capacidad de inversión privada (riqueza no gastada de los capitalistas).
4) DEMANDA Y NIVEL DE VIDA: cada clase demanda mercancías por jerarquía (comida primero, luego ropa/herramientas, luego muebles/papel, lujo si es rica). Nivel de vida 0-100 por clase; si no puede comprar, baja y hay hambruna (población cae).
5) PRESUPUESTO: impuestos por clase (nulo/bajo/medio/alto) con eficiencia según alfabetización y burocracia. Gastos: administración (capacidad de gobernanza), ejército, marina, educación, sanidad, bienestar, infraestructura, corte (gusta a élites). Déficit acumula deuda con intereses; puedes pedir préstamos o declarar suspensión de pagos (penaliza crédito).
6) POLÍTICA: cada clase tiene % de poder político. Leyes simplificadas: censo electoral, impuesto al consumo, derechos laborales, política arancelaria, separación iglesia-estado. Grupos de interés (terratenientes, capitalistas, obreros, campesinos, militares, clero) reaccionan: si les suben impuestos, atacan sus leyes o su nivel de vida cae → descontento → radicales → revolución (game over) o golpe (cambio de gobierno forzado).
7) INVESTIGACIÓN: puntos de investigación mensuales (base + universidades + alfabetización). Árbol de ~25 tecnologías en 3 ramas (producción, sociedad, militar) que desbloquean edificios/leyes y mejoran eficiencia. El jugador elige qué investigar.
8) GRANDES POTENCIAS: ranking de 7 potencias (Valoria + 6 ficticias) por prestigio; el prestigio sube con PNB, ejército y victorias en eventos.
9) EVENTOS: ~20 eventos con 2-3 opciones y condiciones de disparo (mala cosecha, cólera, crisis financiera, huelga general, fiebre del oro, demanda de sufragio, primavera de los pueblos, boom ferroviario...).
10) GUARDADO: localStorage con 3 ranuras + autoguardado anual.

UI
- Barra superior: fecha, velocidad, tesoro, ingresos/gastos, capacidad de inversión, puntos de investigación, prestigio, estabilidad, botones guardar/cargar/ayuda.
- Pestañas laterales: Resumen (dashboard + 2 gráficas de líneas en canvas: PNB y población/precios), Economía (tabla de mercado con oferta/demanda/precio + lista de edificios con botón construir y ver niveles construidos), Población (tarjetas por clase), Política (leyes + grupos de interés + descontento), Investigación (árbol de tecnologías), Presupuesto (sliders de impuesto por clase + asignación de gastos con barras + deuda/préstamos), Grandes Potencias (ranking), Eventos (modal al dispararse).
- Tooltips explicativos en cada mecánica. Gráficas en canvas sin librerías.
- Modal breve de "Cómo jugar" al primer arranque.

SIMULACIÓN (tick mensual, en este orden)
1 producción (los edificios consumen insumos y producen, limitados por insumos y mano de obra), 2 actualización de precios por oferta/demanda, 3 la población compra mercancías y se reparte renta (salarios + beneficios), 4 recaudación de impuestos, 5 efectos del gasto público, 6 nivel de vida, descontento, crecimiento poblacional, 7 investigación, 8 eventos, 9 comprobación de victoria/derrota.

CALIDAD
- Números balanceados para que sea jugable: tesoro inicial ~50.000, primeras décadas ajustadas, el juego debe "engranar" si inviertes bien.
- Código organizado en secciones comentadas.
- Al terminar: extrae el <script> a un temporal y ejecuta `node --check` para confirmar que no hay errores de sintaxis. Corrige cualquier error antes de dar por acabado el trabajo.
