# MOEVS Bike — 3D scroll prototype

Single-page prototype met 3D-fiets in een pinned scroll-sectie en
hotspot-panels die meelopen met scroll-progress.

## Stack

- Plain HTML/CSS/JS (geen build-stap)
- [Three.js 0.160](https://threejs.org/) via ESM import-map (CDN)
- [GSAP 3.12 + ScrollTrigger](https://gsap.com/)
- [Lenis 1.0](https://lenis.studiofreight.com/) voor smooth scroll
- Fonts: Fraunces (italic display) + JetBrains Mono (UI)

Alle dependencies via CDN — niets te installeren.

## Lokale dev-server starten

`<model-viewer>` en `GLTFLoader` werken niet over `file://` (CORS).
Start een mini-webserver in deze map:

```bash
# Python 3
python3 -m http.server 8000

# of Node
npx serve .
```

Open daarna [http://localhost:8000](http://localhost:8000).

## Eigen GLB integreren

Plaats `bike.glb` in deze map. De loader pakt hem automatisch op.
Als het bestand ontbreekt, valt de scene terug op een primitieven-
placeholder (cilinders + torussen).

Het auto-fit-mechanisme (`fitModel()` in `index.html`, regel ±330)
schaalt elk model naar een hoogte van ±2.2 units en plaatst het op
de oorsprong, dus oriëntatie/scale van je export maakt niet uit.

Aanbevolen: Draco-gecomprimeerd, < 10 MB voor snelle laadtijd.
De `DRACOLoader` is al ingehaakt op de Google CDN-decoder.

## Aanpassen

| Wat | Waar | Regel |
|---|---|---|
| Accentkleur | `:root --accent` | ±20 |
| Fonts | `<link>` Google Fonts | ±10 |
| Camera-keyframes | `state` object + ScrollTrigger `onUpdate` | ±455 / ±520 |
| Hotspot-tekst | `.spec-panel` artikelen | ±230–290 |
| Hotspot-posities | `.spec-panel--tl/tr/bl/br/c` CSS classes | ±170 |
| Scroll-lengte | `.scene-container { height: 600vh }` | ±105 |
| Lighting | `key`/`rim`/`fill` directional lights | ±360 |

## Fases

- **Fase 1** ✅ Setup, hero, pinned scene, placeholder choreography
- **Fase 2** ⏳ Volledige scroll-timeline met 5 keyframe-momenten
- **Fase 3** ⏳ Hotspot panels gekoppeld aan scroll-progress
- **Fase 4** ⏳ Polish (loader, hover, ringen, perf)

## GLB-versies in deze map

| File | Grootte | Notes |
|---|---:|---|
| `bike.glb` | 37 MB | Actieve versie (= aggressive-20) |
| `bike-aggressive-20.glb` | 37 MB | Draco + WebP 512 + simplify 0.2 |
| `bike-draco.glb` | 52 MB | Draco + WebP 1024, geen simplify |
| `bike-meshopt.glb` | 203 MB | Mislukt — meshopt incompatibel met tiled UVs |
| `bike-notex.glb` | 16 MB | Geometrie-only, voor grootte-referentie |

Verdere optimalisatie via Blender (decimate modifier) volgt in
een latere stap.
