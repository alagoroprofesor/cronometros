# Cronómetros por etapas

Aplicación web de una sola página para gestionar **N cronómetros parciales más un cronómetro global**, pensada para cronometrar exámenes, pruebas prácticas, presentaciones o cualquier actividad dividida en fases.

Toda la configuración se carga desde un fichero **XML**. No necesita instalación ni servidor: basta con abrir `cronometros.html` en el navegador.

![Captura de la aplicación](Captura%20de%20pantalla%202026-06-13%20140451.png)

## Cómo funciona

- El **cronómetro global** es el tiempo total disponible. Corre en cuenta atrás mientras haya una etapa activa y en marcha.
- Cada **etapa (parcial)** tiene su propia cuenta atrás. Al entrar en una etapa (pulsando su tarjeta o con el teclado) empieza a descontar.
- Al **cambiar de etapa**, el tiempo restante de la anterior se conserva: si vuelves a ella, continúa por donde estaba.
- Cada etapa tiene un **botón de reinicio (↺)** que la devuelve a su tiempo total sin afectar a las demás. Opcionalmente, ese tiempo recuperado puede sumarse también al global (ver `parcialrecuperaglobal`).
- Cuando una parcial llega a 0 se marca como terminada y suena un aviso; el global sigue contando.
- Cuando el global llega a 0 se detiene todo.

## Uso

1. Abre `cronometros.html` en cualquier navegador.
2. Arranca con la configuración por defecto o pulsa **Cargar XML** para usar la tuya.
3. Pulsa una etapa para activarla, o usa los botones **Anterior / Siguiente**.
4. Usa el botón **↺** de cada tarjeta para reiniciar esa etapa concreta.

### Atajos de teclado

- `Espacio` — pausar / reanudar
- `←` / `→` (o `P` / `N`) — etapa anterior / siguiente
- `1`–`9` — ir directamente a una etapa
- `R` — reiniciar todo

## Formato del XML

```xml
<cronometros>
  <global unidad="m">15</global>
  <parciales aviso="10" continuo="1" ciclico="0" parcialrecuperaglobal="0">
    <parcial id="1" unidad="s">30</parcial>
    <parcial id="2" unidad="s" aviso="20">90</parcial>
  </parciales>
</cronometros>
```

### `<global>`

El tiempo total. El texto es el valor numérico y `unidad` indica cómo interpretarlo.

| Atributo | Valores | Por defecto | Descripción |
|----------|---------|-------------|-------------|
| `unidad` | `s`, `m`, `h` | `s` | Segundos, minutos u horas. |

### `<parciales>`

Contenedor de las etapas. Sus atributos fijan el comportamiento por defecto del grupo.

| Atributo | Valores | Por defecto | Descripción |
|----------|---------|-------------|-------------|
| `aviso` | número (segundos) | `5` | Umbral en segundos a partir del cual la etapa se muestra en rojo. Se puede sobreescribir en cada `<parcial>`. |
| `continuo` | `1` / `0` | `0` | Con `1`, al acabar una etapa salta automáticamente a la siguiente que aún tenga tiempo. |
| `ciclico` | `1` / `0` | `0` | Con `1`, del último parcial se pasa al primero (tanto en el salto automático como en los botones). Con `0` no se da la vuelta. |
| `parcialrecuperaglobal` | `1` / `0` | `0` | Con `1`, al reiniciar una etapa con el botón ↺, los segundos que recupera esa parcial (`total − restante`) se suman también al cronómetro global, sin superar nunca su total. Con `0` el global no se modifica. |

### `<parcial>`

Cada etapa. El texto es el valor numérico.

| Atributo | Valores | Por defecto | Descripción |
|----------|---------|-------------|-------------|
| `id` | texto | posición | Identificador mostrado en la etapa. |
| `unidad` | `s`, `m`, `h` | `s` | Unidad del valor. |
| `aviso` | número (segundos) | el del grupo | Umbral de aviso propio de esta etapa. |

### Regla de validación

La **suma de las parciales no puede superar el tiempo global**. Si se supera, el fichero no se carga y se muestra un aviso indicando ambos totales.

## Licencia

Uso educativo libre.
