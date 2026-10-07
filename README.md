# Generador de Paletas

Herramienta web para generar paletas de color armónicas, explorar una galería infinita de paletas y guardar las favoritas. JavaScript puro — sin frameworks, sin dependencias, sin paso de compilación.

**Demo en vivo:** https://tu-dominio.netlify.app/demo-generador-paletas.html
> Al probarlo abriendo `index.html` con doble clic, la pestaña "Explorar" seguirá funcionando igual (es `fetch` normal, no un módulo ES), pero necesitas conexión a internet para ver paletas "en vivo" — sin red, cae automáticamente al generador propio.
**Más contexto del proyecto:** https://tu-dominio.netlify.app/proyectos/generador-paletas

![Pestaña Generar](./docs/screenshot-generar.png)
![Pestaña Explorar](./docs/screenshot-explorar.png)

## Características

**Generar**
- Paletas de 3 a 7 colores según distintas reglas de armonía: aleatoria, análoga, complementaria, tríada, monocromática y pastel.
- Bloqueo de colores individuales para conservarlos mientras se regeneran el resto.
- Copiar el HEX de un color con un clic, o exportar la paleta completa como variables CSS.
- Atajo de teclado: la barra espaciadora regenera los colores no bloqueados.

**Explorar**
- Galería de scroll infinito que combina tres fuentes — nunca se acaba: una colección curada de paletas reales, paletas en vivo pedidas a [The Color API](https://www.thecolorapi.com) (pública, sin clave) y generación algorítmica propia cuando la red falla o tarda.
- Filtros por estilo: cálidas, frías, pastel, oscuras, vivas, tierra.
- Guardar cualquier paleta con un clic en la estrella.

**General**
- Paletas guardadas persistentes en `localStorage`, compartidas entre ambas pestañas.
- Sin backend propio ni clave de API: la única llamada de red es a un servicio público gratuito, y si no responde en 4 segundos (o el visitante está offline) la app genera la paleta por su cuenta sin que se note.

## Stack técnico

- HTML5 + CSS3 + JavaScript moderno (ES2020+: `const`/`let`, funciones flecha, `async`/`await`, desestructuración), sin build ni frameworks
- `fetch` + `AbortController` para pedir paletas a [The Color API](https://www.thecolorapi.com/docs) con timeout y `Promise.allSettled` para no bloquear el lote si una petición falla
- Conversión de color HSL → HEX escrita a mano
- Color del texto elegido con la fórmula de contraste de WCAG (luminancia relativa), para que el código HEX se lea sobre cualquier fondo
- `localStorage` con manejo de errores (try/catch) para navegación privada

## Uso local

Al ser un único archivo HTML sin dependencias, basta con abrirlo directamente:

```bash
# clonar y abrir index.html en el navegador
git clone https://github.com/HookMike/generador-paletas.git
cd generador-paletas
# doble clic en index.html, o:
python3 -m http.server 8000
```

## Por qué está hecho así

Quise demostrar que se puede construir una herramienta pulida y realmente útil sin depender de un framework ni de un backend propio. La galería "Explorar" sí llama a una API pública de color para traer paletas reales, pero nunca depende de ella al 100 %: agota primero una colección curada (resultados instantáneos y con buen gusto garantizado desde el primer segundo), y si la API no responde a tiempo —sin conexión, caída del servicio, límite de peticiones— cae de forma silenciosa a un generador algorítmico propio basado en rangos HSL por categoría. Así la demo nunca se "rompe" ni se queda esperando a un tercero, pase lo que pase con la red.

La app sigue sin build ni paso de compilación a propósito: todo el JavaScript va dentro de `index.html` en un `<script>` normal, no como módulo ES. Así se puede abrir con doble clic, sin montar un servidor, porque los navegadores bloquean los scripts `type="module"` cargados desde `file://`.

## Autor

Miguel Ángel Alonso Valhondo — [LinkedIn](https://www.linkedin.com/in/miguelangel-alonso-valhondo) · [Portfolio](https://tu-dominio.netlify.app)
