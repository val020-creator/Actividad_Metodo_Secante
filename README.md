# Actividad: Métodos numéricos para ecuaciones no lineales

**Autor:** Valery Montes Echavez
**Curso:** Métodos Númericos
**Docente:** Caterine Diaz

## Descripción

Este repositorio contiene la solución de la actividad de búsqueda de raíces de ecuaciones no lineales. Se trabaja principalmente con el **método de la secante** y se compara con **Newton-Raphson**. Además, incluye una herramienta web (`metodos_numericos.html`) que genera las tablas de iteraciones para cada función y método.

## Contenido del repositorio

```
├── README.md                    # Este documento
├── metodos_numericos.html       # Herramienta interactiva (abrir en el navegador)
├── secante_excel.xlsx           # Punto c: secante en Excel (agregar tu archivo)
└── newton_excel.xlsx            # Punto d: Newton-Raphson en Excel (agregar tu archivo)
```

> Ajusta los nombres de los archivos `.xlsx` a los que realmente subas.

## Ejercicios

| # | Función | Puntos iniciales |
|---|---|---|
| 1 | f(x) = ln(x) − 2x + 3 | x₀ = 1, x₁ = 2 (elegidos por el análisis del punto a) |
| 2 | f(x) = x³ − 3x − 4 | x₀ = 5, x₁ = 7 |
| 3 | f(x) = x² − x + 6 | x₀ = 1, x₁ = 2 |
| 4 | f(x) = 2·sen(x) − x | x₀ = 1, x₁ = 2 |

---

## Punto b: convergencia del método de la secante

**Fórmula**

```
x(n+1) = x(n) − f(x(n)) · (x(n) − x(n−1)) / (f(x(n)) − f(x(n−1)))
```

**Orden de convergencia:** si f es suficientemente derivable, f′(r) ≠ 0 y los puntos iniciales están cerca de la raíz r, el error cumple

```
|e(n+1)| ≈ | f″(r) / (2 f′(r)) |^0.618 · |e(n)|^1.618
```

El orden es p = (1 + √5) / 2 ≈ **1.618** (razón áurea), es decir, convergencia **superlineal**: más rápida que la bisección (orden 1) y algo más lenta que Newton-Raphson (orden 2). No necesita calcular f′(x) y solo evalúa f una vez por iteración.

**Condiciones y riesgos**

- La convergencia es **local**: con puntos iniciales lejanos puede divergir.
- Si f(xₙ) ≈ f(xₙ₋₁), el denominador se anula y el método falla.
- No exige que la raíz quede encerrada entre x₀ y x₁.
- Si la función no tiene raíz real, no converge.

---

## Punto c: método de la secante en Excel

Herramienta distinta de Python: **Microsoft Excel**.

Tabla utilizada: `i | X0 | X1 | f(X0) | f(X1) | Xi | f(Xi) | |Xi − Xi−1|`

**Fórmulas (fila 2 con los valores iniciales en B2 y C2, y fila 3 para arrastrar):**

| Celda | Fila 2 | Fila 3 (arrastrar) |
|---|---|---|
| A | `1` | `=A2+1` |
| B | x₀ | `=C2` |
| C | x₁ | `=F2` |
| D | f(B2) | `=E2` |
| E | f(C2) | f(C3) |
| F | `=C2-E2*(C2-B2)/(E2-D2)` | `=C3-E3*(C3-B3)/(E3-D3)` |
| G | f(F2) | f(F3) |
| H | `=ABS(F2-C2)` | `=ABS(F3-C3)` |

**f(x) en Excel (ejemplo para la celda B2)**

| Ecuación | Fórmula |
|---|---|
| 1) ln | `=LN(B2)-2*B2+3` |
| 2) cúbica | `=B2^3-3*B2-4` |
| 3) cuadrática | `=B2^2-B2+6` |
| 4) seno | `=2*SENO(B2)-B2` |

(En Excel en inglés usar `SIN` en lugar de `SENO`.)

### Resultados

| Ecuación | Raíz aproximada | Iteraciones (tol. 10⁻¹⁰) | Observación |
|---|---|---|---|
| 1) ln(x) − 2x + 3 | **1.7915369380** | 5 | Converge rápido. Existe otra raíz cerca de 0.0526. |
| 2) x³ − 3x − 4 | **2.1958233454** | 10 | Converge, lento al inicio por puntos lejanos a la raíz. |
| 3) x² − x + 6 | — | — | **No converge**: discriminante 1 − 24 < 0, no hay raíces reales. |
| 4) 2·sen(x) − x | **1.8954942670** | 6 | Converge rápido (hay cambio de signo entre 1 y 2). |

---

## Punto d: Newton-Raphson sobre f(x) = 2·sen(x) − x

**Fórmula**

```
x(n+1) = x(n) − (2·sen(x(n)) − x(n)) / (2·cos(x(n)) − 1)
```

con f′(x) = 2·cos(x) − 1.

Tabla: `i | X(i−1) | f(X(i−1)) | f′(X(i−1)) | Xi | f(Xi) | |Xi − Xi−1|`

| Celda | Fila 2 | Fila 3 (arrastrar) |
|---|---|---|
| A | `1` | `=A2+1` |
| B | x₀ | `=E2` |
| C | `=2*SENO(B2)-B2` | `=2*SENO(B3)-B3` |
| D | `=2*COS(B2)-1` | `=2*COS(B3)-1` |
| E | `=B2-C2/D2` | `=B3-C3/D3` |
| F | `=2*SENO(E2)-E2` | `=2*SENO(E3)-E3` |
| G | `=ABS(E2-B2)` | `=ABS(E3-B3)` |

### Resultados

| x₀ | Resultado |
|---|---|
| **2** | Converge a **1.8954942670** en 4 iteraciones. |
| **1** | **Diverge**: f′(1) ≈ 0.0806 es casi cero, el primer paso salta a x₁ ≈ −7.47 y los iterados oscilan sin control. |

### Comparación con la secante (misma raíz)

| | Secante | Newton-Raphson (x₀ = 2) |
|---|---|---|
| Iteraciones para ~10 cifras | 6 | 4 |
| Orden de convergencia | 1.618 | 2 |
| Necesita f′(x) | No | Sí |
| Evaluaciones de función por iteración | 1 | 2 (f y f′) |
| Robustez con los datos dados | Converge con x₀ = 1, x₁ = 2 | Falla con x₀ = 1 |

**Conclusión:** Newton-Raphson converge en menos iteraciones, pero exige derivar y es frágil cuando f′(xₙ) ≈ 0. La secante necesita algunas iteraciones más, pero no requiere derivada y fue más robusta con los puntos dados. En costo total son comparables, porque la secante evalúa f una sola vez por iteración.

> Nota: 2·sen(x) − x también tiene la raíz trivial x = 0 y, por simetría, x ≈ −1.8955.

---

## Herramienta web: `metodos_numericos.html`

Aplicación de un solo archivo (HTML + CSS + JavaScript, sin dependencias) que genera la tabla de iteraciones.

### Uso

1. Abre `metodos_numericos.html` en cualquier navegador (doble clic).
2. Elige la **función**: las 4 de la actividad.
3. Elige el **método**: Secante, Newton-Raphson o Bisección.
4. Ajusta si quieres los valores iniciales, la tolerancia, el máximo de iteraciones y los decimales.
5. Pulsa **Calcular tabla**. La tabla también se actualiza al cambiar la función o el método.
6. Pulsa **Copiar para Excel** y pega en la celda A1 de una hoja.

### Opciones

| Opción | Descripción |
|---|---|
| Valores iniciales | Se cargan los de la actividad al cambiar de función; son editables. En Newton solo se usa x₀. |
| Tolerancia | Criterio de parada: `|f(xᵢ)| < tol` o `|xᵢ − xᵢ₋₁| < tol` (por defecto 10⁻¹⁰). |
| Máx. iteraciones | Límite de seguridad (por defecto 15, máximo 100). |
| Separador decimal | Punto o coma, para pegar sin conversiones en un Excel con configuración regional en español. |

### Columnas por método

- **Secante:** `i | X0 | X1 | f(X0) | f(X1) | Xi | f(Xi) | |Xi - Xi-1|`
- **Newton-Raphson:** `i | X(i-1) | f(X(i-1)) | f′(X(i-1)) | Xi | f(Xi) | |Xi - Xi-1|`
- **Bisección:** `i | a | b | f(a) | f(b) | c | f(c) | |c - c anterior|`

### Avisos que muestra

- Convergencia lograda, con la raíz aproximada y el número de iteraciones.
- Falta de convergencia dentro del máximo de iteraciones (por ejemplo, con x² − x + 6).
- Derivada casi cero en Newton, o denominador cero en la secante.
- Bisección sin cambio de signo en el intervalo.

### Cómo agregar otra función

En el bloque `const F = {...}` del script, añade una entrada con `f`, su derivada `d` y los puntos iniciales `a` y `b`. Después agrega una `<option>` al selector `#fn`.

---

## Cómo subir el proyecto a GitHub

```bash
git init
git add .
git commit -m "Actividad métodos numéricos: secante, Newton-Raphson y herramienta web"
git branch -M main
git remote add origin https://github.com/<usuario>/<repositorio>.git
git push -u origin main
```
