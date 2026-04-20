# Space Shooter — Documentación de prompts

**Herramienta:** Claude Sonnet 4.6  
**Modelo ID:** claude-sonnet-4-6  
**Fecha:** 2026-04-20

---

## Índice

- [Prompt 1 — Análisis y elección de tecnología](#prompt-1--análisis-y-elección-de-tecnología)
- [Prompt 2 — Estructura y diseño del juego](#prompt-2--estructura-y-diseño-del-juego)
- [Prompt 3 — Implementación completa](#prompt-3--implementación-completa)

---

## Prompt 1 — Análisis y elección de tecnología

**Prompt enviado:**

```text
Quiero implementar un videojuego para el repositorio AI4Devs-videogame-202602.
Analiza la diferencia de complejidad entre un Space Shooter clásico y un juego
tipo Spider (Qix-style, reclamación de territorio). ¿Cuál tecnología recomiendas,
Phaser 3 o vanilla JS + Canvas?
```

**Respuesta clave:** Se eligió **vanilla JS + Canvas** sin dependencias externas porque:

- Funciona directamente desde `file://` sin errores CORS
- No requiere servidor local (a diferencia de Phaser 3)
- Suficiente para un Space Shooter de 5 niveles
- Un único archivo HTML autocontenido, igual que el ejemplo `snake-EHS`

---

## Prompt 2 — Estructura y diseño del juego

**Prompt enviado:**

```text
Vamos a hacer el Space Shooter. Diseña la estructura del juego con los siguientes
requisitos:
- Nave del jugador dibujada con Canvas API (sin imágenes externas)
- Formación de enemigos tipo Space Invaders que avanza
- Disparos del jugador (Space/W/ArrowUp) y disparos enemigos aleatorios
- 5 niveles con dificultad creciente (velocidad y cadencia de disparo)
- Partículas de explosión
- Fondo estrellado animado
- HUD con puntuación, nivel y vidas
- Game Over y pantalla de victoria
- Todo en un único index.html sin dependencias
```

**Decisiones de diseño adoptadas:**

| Elemento | Decisión |
|---|---|
| Gráficos | Canvas 2D API, formas geométricas (sin imágenes) |
| Controles | Flechas + WASD + Space |
| Enemigos | Grid 10×4, color por fila, puntuación inversa a la fila |
| Dificultad | `enemySpeed` y `enemyShootInterval` escalan por nivel |
| Vidas | 3 vidas, flash de daño al recibir impacto |
| Niveles | 5 niveles; al completar el 5 muestra pantalla de victoria |

---

## Prompt 3 — Implementación completa

**Prompt enviado:**

```text
Implementa el Space Shooter completo en un único index.html con vanilla JS y Canvas.
Asegúrate de que:
- La nave se dibuja con paths Canvas (triángulo + alas + propulsores con llama animada)
- Los enemigos tienen forma de nave invertida con antenas y ojos
- Las explosiones usan sistema de partículas con física básica (gravedad leve)
- El fondo tiene 120 estrellas de diferentes tamaños y velocidades
- El HUD usa iconos ▲ para representar las vidas
- Los overlays (menú, nivel, game over, victoria) son accesibles y responsivos
- No hay dependencias externas ni llamadas a red
```

**Resultado:** Juego funcional en un único archivo `index.html` (~350 líneas).

**Características implementadas:**

- Fondo estrellado con paralaje sutil (120 estrellas, velocidades variadas)
- Nave del jugador con propulsores con llama animada (color aleatorio por frame)
- Grid de enemigos 10×4 con 6 colores según fila
- Sistema de partículas para explosiones (vida, velocidad, gravedad y decaimiento)
- Flash de color rojo en la nave al recibir daño (30 frames)
- Velocidad y cadencia de disparo enemigo escalan con el nivel
- Detección de colisión AABB para balas y nave
- Pantallas de overlay: Menú → Juego → Nivel siguiente → Game Over / Victoria

---

## Controles

| Tecla | Acción |
|---|---|
| `←` / `A` | Mover izquierda |
| `→` / `D` | Mover derecha |
| `Space` / `W` / `↑` | Disparar |

---

## Cómo jugar

1. Abre `index.html` directamente en el navegador (no requiere servidor)
2. Pulsa **Iniciar**
3. Elimina todos los enemigos de cada nivel para avanzar
4. Evita los disparos enemigos (tienes 3 vidas)
5. Supera los 5 niveles para ganar

---

## Retos y soluciones

| Reto | Solución |
|---|---|
| Propulsores con llama animada | Elipsis con color aleatorio por frame (`Math.random()` en cada `draw`) |
| Escalado de dificultad equilibrado | `enemySpeed = 0.8 + level * 0.35` y `enemyShootInterval = max(40, 100 - level*10)` |
| Detección de borde para inversión de marcha | `Math.min/max` sobre el array de enemigos vivos |
| Evitar CORS | Solo Canvas API, sin carga de assets externos |
