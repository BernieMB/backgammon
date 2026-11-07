# Backgammon

Juego de backgammon en un único archivo HTML autocontenido (`backgammon.html`). No necesita instalación, servidor ni conexión a internet: basta con abrirlo en un navegador moderno (Chrome, Edge, Firefox, Safari).

## Características

- **1 jugador** contra el ordenador o **2 jugadores** en el mismo equipo.
- **Tres niveles de dificultad** para el ordenador: Fácil, Normal y Difícil.
- **Cuatro temas** de tablero: Madera, Casino, Medianoche y Arena (se pueden cambiar durante la partida).
- Reglas completas: tirada inicial, dobles, golpes y barra, reentrada obligatoria, sacar fichas y uso obligatorio de los dados.
- Marcador acumulado con victoria simple (1), gammon (2) y backgammon (3) y opción de revancha.
- Página de instrucciones integrada.
- No incluye cubo de doblar ni partidas guardadas.

## Diseño

Todo el código (HTML, CSS y JavaScript) está en `backgammon.html`, sin dependencias externas.

- **Pantallas**: menú inicial, partida e instrucciones. Son secciones del mismo documento que se muestran u ocultan.
- **Motor de reglas** (entre los marcadores `/*ENGINE-START*/` y `/*ENGINE-END*/`): independiente de la interfaz, por lo que puede extraerse y probarse en Node.
  - Estado: `pts[24]` con signo (positivo = jugador 1, negativo = jugador 2), `bar[2]` y `off[2]`.
  - `legalMoves` calcula los movimientos individuales permitidos respetando la obligación de usar el máximo número de dados y el dado mayor.
  - `genSeqs` genera todas las secuencias completas de un turno (usadas por la IA).
- **Inteligencia artificial**:
  - *Fácil*: algo más de la mitad de las veces elige una jugada al azar.
  - *Normal*: elige la jugada mejor valorada por una función de evaluación (carrera de pips, puntos construidos, bloqueos, fichas expuestas, fichas en la barra).
  - *Difícil*: usa pesos de evaluación ajustados mediante partidas de auto-juego y anticipa la mejor respuesta del rival para cada una de las 21 tiradas posibles.
- **Interfaz**: tablero dibujado en SVG con animaciones de movimiento. Los colores de cada tema se definen con variables CSS.
- Los ajustes del menú (modo, dificultad, nombres, tema) se recuerdan en `localStorage`.

## Cómo se juega

### Empezar

1. Abre `backgammon.html` en el navegador.
2. En el menú elige el modo (1 o 2 jugadores), la dificultad, los nombres y el tema.
3. Pulsa **Nueva partida**.

### Objetivo

Cada jugador tiene 15 fichas. Hay que llevarlas todas a tu **casa** (tu último cuadrante de 6 puntos) y después **sacarlas** del tablero. Gana quien saque primero sus 15 fichas.

- **Jugador 1** (fichas claras): casa abajo a la derecha, mueve en sentido antihorario.
- **Jugador 2 / Ordenador** (fichas oscuras): casa arriba a la derecha, mueve en sentido horario.

### Reglas básicas

- **Inicio**: cada jugador tira un dado; el más alto empieza y usa ambos dados como primera tirada.
- **Mover**: cada dado es un movimiento. Puedes mover a un punto vacío, con fichas propias o con una sola ficha rival. Un punto con dos o más fichas rivales está bloqueado.
- **Dobles**: se juega el número cuatro veces.
- **Uso obligatorio**: hay que usar ambos dados si es posible; si solo se puede usar uno, debe ser el mayor.
- **Golpear**: si caes en un punto con una sola ficha rival, la mandas a la **barra**. Quien tiene fichas en la barra debe reentrarlas por la casa del rival antes de mover cualquier otra.
- **Sacar fichas**: solo cuando las 15 están en casa. Si no hay ficha en el punto del dado y no quedan fichas en puntos más altos, se saca la del punto más alto ocupado.

### Puntuación

| Resultado   | Condición                                                        | Puntos |
|-------------|------------------------------------------------------------------|--------|
| Simple      | El rival ya ha sacado alguna ficha                               | 1      |
| Gammon      | El rival no ha sacado ninguna ficha                              | 2      |
| Backgammon  | Como gammon, y además el rival tiene fichas en la barra o en tu casa | 3  |

### Controles

1. Pulsa **Tirar dados** (botón inferior o el del tablero).
2. Haz clic en una ficha que se pueda mover (marcada con un anillo discontinuo).
3. Haz clic en uno de los destinos iluminados. Para sacar una ficha, pulsa la **bandeja** iluminada de la derecha.
4. Cuando termines, pulsa **Terminar turno** (o haz clic en los dados). En el menú puedes activar el fin de turno automático.
5. **Deshacer** revierte los movimientos del turno actual mientras no lo hayas terminado.

| Tecla     | Acción                         |
|-----------|--------------------------------|
| `Espacio` | Tirar dados / terminar turno   |
| `Z`       | Deshacer                       |
| `Esc`     | Deseleccionar ficha            |

En los paneles superiores se muestra el **recuento de pips** de cada jugador (lo que le falta para sacar todas sus fichas), las fichas sacadas y el marcador.

La explicación completa de las reglas y algunos consejos de estrategia están en la pantalla **Instrucciones** del propio juego.
