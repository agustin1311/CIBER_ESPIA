# CYBER ESPÍA: OPERACIÓN VIRUS (versión Godot)

Juego 2D de plataformas y disparos. Controlas a la espía de pelo celeste y antenas e implantas
un virus cibernético en el servidor de 3 equipos enemigos: Crimson, Solaris y Nova.

Port a **Godot 4.3+** (GDScript) del original en C++ / raylib.

## Cómo abrirlo

1. Instala [Godot 4.3 o superior](https://godotengine.org/download) (versión estándar, no hace falta .NET).
2. En el gestor de proyectos pulsa **Importar** y selecciona el archivo `project.godot`.
3. Pulsa **F5** (Ejecutar). La escena principal es `scenes/main.tscn`.

La primera vez Godot importa los sprites de `assets/`; es automático.

## Controles

Las acciones están en **Proyecto > Configuración del proyecto > Mapa de entrada**, así que
puedes cambiarlas sin tocar código.

| Acción (InputMap) | Teclas |
|---|---|
| `move_left` / `move_right` | A / D (o flechas) |
| `jump` | W, flecha arriba o Espacio |
| `drop` | S o flecha abajo (bajar de las plataformas) |
| `shoot` | J o clic izquierdo (el clic apunta al mouse) |
| `cloak` | SHIFT o CTRL: sigilo (casco), gasta energía. Disparar lo rompe |
| `hack` | E (mantener): implantar el virus en el servidor |
| `confirm` | ENTER: menú y pantallas de fin |

## Cómo se juega

- Los paquetes de virus **infectan** a los enemigos: con 2 impactos quedan infectados (verde).
- Un enemigo infectado **ataca a sus propios compañeros**; a los 6 s se cae el sistema (+150 puntos).
- Llega al servidor al final del nivel y mantén **E** hasta completar la barra. Si te disparan, pierdes progreso.
- 3 misiones, cada una más larga y con más defensas.

## Estructura del proyecto

```
espia_virus_godot/
├── project.godot          # configuración, InputMap, ventana 1280x720
├── assets/                # front.png, side.png, helmet.png
├── scenes/
│   └── main.tscn          # Game > Background / World / Camera2D / HUDLayer
└── scripts/
    ├── game.gd            # lógica: estados, niveles, jugador, enemigos, balas, hackeo
    ├── world_view.gd      # dibuja el mundo (suelo, plataformas, enemigos, jugador, balas)
    ├── background.gd      # fondo con parallax y lluvia de código
    ├── hud.gd             # HUD, menú y pantallas de victoria / derrota
    └── util.gd            # helpers de dibujo (texto, elipses, barras, hash)
```

## Equivalencias con el original (raylib → Godot)

| raylib / C++ | Godot / GDScript |
|---|---|
| `Game::Update` / `Draw` | `_process()` en `game.gd` + `_draw()` en los nodos de vista |
| `Camera2D` de raylib | nodo `Camera2D` (posición suavizada a mano) |
| `IsKeyDown(KEY_...)` | `Input.is_action_pressed("accion")` |
| `GetScreenToWorld2D(mouse)` | `get_global_mouse_position()` |
| `DrawRectangle`, `DrawCircle`, `DrawText` | `draw_rect`, `draw_circle`, `draw_string` |
| `std::vector<Bullet>` | `Array` de clases internas (`Bullet`, `Enemy`, `Particle`) |
| `CheckCollisionRecs` | `Rect2.intersects()` |

## Cambios respecto al original

- Los textos ya llevan tildes y eñ (la fuente de Godot las soporta).
- Las ventanas de los edificios del fondo ya no parpadean al mover la cámara.
- La torreta ahora apunta hacia su objetivo (antes siempre miraba a la derecha).
- Además de S, la flecha abajo también sirve para bajar de las plataformas.

## Siguientes pasos sugeridos (para llevarlo a "estilo Godot")

El port conserva el dibujo por código para mantener el aspecto idéntico. Si quieres aprovechar
más el motor: convertir al jugador en `CharacterBody2D` con `Sprite2D`, los enemigos en una
escena reutilizable (`enemy.tscn`) con `Area2D`, las plataformas en `StaticBody2D` /
`TileMap`, y las balas en una escena instanciada; el HUD en nodos `Control`.
