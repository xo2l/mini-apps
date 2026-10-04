# Mini Apps

Small single-page web tools. Each app is one self-contained `index.html` in its own folder: open it in a browser, nothing to install or build.

| App | What it does |
|---|---|
| [Spin Stencil](spin-stencil/) | Turns a picture into a rotating colouring stencil: a disk with numbered windows and a base sheet with marks. Exports PDF (A4, 1:1) and SVG for hand cutting or a laser/plotter. |

Every app has an English interface by default with a switch to Russian.

## Spin Stencil

You put the stencil disk on the base sheet with a pin through the centre, turn it to mark 1, colour every window labelled 1, turn to mark 2, and so on. Each window is a piece of the picture rotated by its own angle, so the stencil looks like random shapes and the picture only appears once all positions are done.

How the generator works:

1. The picture is reduced to a few colours (k-means) and cleaned up.
2. In **Outline** mode each area becomes a band along its inner edge; in **Fill** mode areas are used whole.
3. The painted area is cut into fragments (jittered Voronoi cells).
4. Each fragment gets a turn position so that its window, rotated back onto the stencil, does not touch any other window closer than the bridge width. Fragments that do not fit are split by polar angle and retried.
5. Window outlines are traced, simplified and smoothed; labels are placed next to their windows and rotated to read upright at their own mark.

Limitation: at any given radius the windows cannot cover the whole circle, or the stencil falls apart. Solid fills of large shapes therefore only partly fit; use Outline mode for those.

Modes:

- **Craft knife** — larger windows, 2.5 mm bridges, printable PDF on two A4 pages (stencil and base). Print at 100 %, without fit-to-page.
- **Laser / plotter** — finer windows, 1.2 mm bridges. In the SVG red strokes are cuts and blue text is engraving; convert text to paths in your laser software.

---

## По-русски

Набор небольших веб-инструментов. Каждое приложение — один `index.html` в своей папке, открывается в браузере, ничего ставить не нужно. По умолчанию интерфейс на английском, переключатель EN/RU в правом верхнем углу.

**Поворотный трафарет** превращает картинку в диск с пронумерованными окнами и основу с метками. Ставишь диск на метку, закрашиваешь окна с этим номером, поворачиваешь дальше. Рисунок проявляется только в конце. Для сплошных крупных форм используй режим «Контур»: на одном радиусе окна не могут занимать весь круг, иначе трафарет развалится.
