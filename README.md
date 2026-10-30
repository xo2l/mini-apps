# Mini Apps

Small single-page web tools. Each app is one self-contained `index.html` in its own folder: open it in a browser, nothing to install or build.

| App | What it does |
|---|---|
| [Spin Stencil](spin-stencil/) | Turns a picture into a rotating colouring stencil: a disk with numbered windows and a base sheet with marks. Exports PDF (A4, 1:1) and SVG for hand cutting or a laser/plotter. |
| [Cadastre Map KZ](cadastre-map/) | Kazakhstan land cadastre (EGKN) layers over a base map of your choice. Draw a plot to get its area, perimeter, coordinate list and a printable plan. |

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

## Cadastre Map KZ

The public cadastre map of Kazakhstan ([map.gov4c.kz/egkn](https://map.gov4c.kz/egkn/)) only offers its own base maps. This app puts the same cadastre layers over any base map: Garysh Sapary (national satellite), Google satellite or hybrid, Esri, Bing, Yandex satellite or map, 2GIS, OpenStreetMap, the EGKN scheme or Kazgeodesy aerial photos.

Layers tab:

- Cadastre layers: registered and requested plots, land auctions, plot parts, arrests, buildings, cadastral blocks, valuation zones, farmland, soil and vegetation surveys, master plan and detailed plan zoning and red lines, utility supply zones, Emergencies Ministry flood extent. Each has its own opacity slider and legend.
- Districts: the cadastre serves plot layers one district at a time, so pick the districts you need. A city and the district around it are separate.
- Click the map for coordinates (WGS 84 and UTM) and a link to the cadastre record of the plot under the point: cadastral number, address, intended use.

Draw & measure tab:

- Draw a plot by points or place a rectangle of a set size (presets from 0.10 to 1 ha); move, rotate, add or delete corners.
- Area in m², ha and sotka; perimeter; side lengths; bearings; a coordinate list numbered clockwise from the north-west corner, in WGS 84 and UTM.
- Printable plan with numbered corners, side lengths, north arrow, scale bar and the coordinate list. Print it or save as PDF.
- Export to KML, GeoJSON and CSV; import KML or GeoJSON. Plots are kept in the browser.

How it works: the cadastre's district layers only render in the district's own UTM zone and come back empty in Web Mercator. The app requests each tile in UTM and warps it onto the map tile, so the layers line up with any base map. Area is computed on the WGS 84 ellipsoid through UTM with the zone scale correction.

Limitations: the official plan for an application is made in the local coordinate system (MSK) by a licensed land surveyor; this app gives WGS 84 and UTM. The cadastre does not allow its records inside other pages, so they open in a new tab. The maps need the internet.

---

## По-русски

Набор небольших веб-инструментов. Каждое приложение — один `index.html` в своей папке, открывается в браузере, ничего ставить не нужно. По умолчанию интерфейс на английском, переключатель EN/RU в правом верхнем углу.

**Поворотный трафарет** превращает картинку в диск с пронумерованными окнами и основу с метками. Ставишь диск на метку, закрашиваешь окна с этим номером, поворачиваешь дальше. Рисунок проявляется только в конце. Для сплошных крупных форм используй режим «Контур»: на одном радиусе окна не могут занимать весь круг, иначе трафарет развалится.

**Кадастровая карта РК** показывает слои ЕГКН (зарегистрированные и испрашиваемые участки, аукционы, учётные кварталы, генплан, обследование земель и др.) поверх любой подложки: «Гарыш Сапары», Google, Яндекс, Bing, Esri, 2ГИС, OSM. Клик по карте даёт координаты и ссылку на сведения ЕГКН об участке. На вкладке «Рисование и площадь» можно нарисовать участок или поставить прямоугольник нужного размера и получить площадь (м², га, сотки), периметр, длины сторон, дирекционные углы, каталог координат и схему для печати. Есть экспорт в KML, GeoJSON и CSV. Координаты даются в WGS 84 и UTM; официальную схему в МСК делает землеустроитель.
