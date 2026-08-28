# CONTEXT — eusynth-market-data

Documento de retome. Destila el estado del repositorio a fecha 2026-08-28.

## 1. Qué es

Dataset abierto de estadísticas agregadas de precios del mercado de segunda mano
de sintetizadores en marketplaces europeos, publicado por el "European Synthesizer
Market Observatory" (proyecto independiente, no comercial; web en
intellisynthprices.com, alias visible "IntelliSynth Prices").

Este repositorio **solo contiene los datos publicados y su documentación**. El
pipeline de scraping, matching y cálculo vive en otro sitio (PENDIENTE DE
CONFIRMAR dónde; presumiblemente el backend de intellisynthprices.com). Aquí se
publican snapshots periódicos bajo licencia CC BY 4.0.

## 2. Stack

- **Contenido:** 3 ficheros CSV en `data/` + `README.md`, `CITATION.cff`
  (formato Citation File Format 1.2.0), `LICENSE` (CC BY 4.0).
- **Sin código:** no hay `pyproject.toml`, `requirements.txt`, `package.json` ni
  scripts. No hay stack de aplicación en este repo.
- **Distribución:** GitHub, repo `git@github.com:albertjimrod/eusynth-market-data`.
  Los consumidores hacen `git pull` / "watch" para recibir nuevas releases.
- **Pipeline de origen (fuera de repo):** recolección ~horaria de marketplaces
  públicos, embargo de 72 h por listing, matching de títulos a nombres canónicos
  con un clasificador ML, y cálculo de Fair Market Price como percentiles sobre
  ventana móvil de 90 días (mín. 5 muestras, outliers excluidos). Detalles y
  lenguaje del pipeline: PENDIENTE DE CONFIRMAR.

## 3. Arquitectura

Tres datasets relacionados por `canonical_name`:

- **`data/products.csv`** — catálogo canónico de productos (README: 3.037
  modelos; snapshot local con menos). Columnas: manufacturer, model,
  canonical_name, total_listings, listings_with_price, first_seen, last_seen,
  `has_fmp` (1 si existe Fair Market Price). Nota README: `total_listings`
  excluye eBay.
- **`data/fair_prices.csv`** — estimaciones de precio para los modelos con
  actividad de mercado suficiente (README: 122; snapshot local con menos).
  Columnas: percentiles `price_p25/p50/p75_eur`, `sample_size`, `window_days`
  (90), `sources` (lista de IDs numéricos de marketplace, p. ej. `[1, 7]`),
  `computed_date`. Subconjunto de `products.csv` (los que tienen `has_fmp = 1`).
- **`data/monthly_market_stats.csv`** — actividad mensual agregada por
  marketplace: month, source, source_country, listings_scraped,
  listings_with_price, avg/min/max_price_eur, distinct_products.

Privacidad por diseño: no se recogen ni almacenan datos de vendedores
(usuarios, nombres, contacto). El contenido individual de listings (títulos,
precios, URLs) no se incluye; queda sujeto a los términos de cada marketplace.

Los IDs de `sources` en `fair_prices.csv` no tienen tabla de mapeo en el repo
(se ve `[1]`, `[7]`, `[1, 7]`). Correspondencia ID→marketplace: PENDIENTE DE
CONFIRMAR.

## 4. Decisiones clave

- **Repo solo-datos, separado del pipeline.** Permite publicar bajo CC BY 4.0
  sin exponer el scraper ni su código/infra.
- **No publicar datos de vendedores ni listings crudos.** Cumplimiento de
  privacidad y de los términos de terceros; se publican solo agregados.
- **Embargo de 72 h** antes de procesar cada listing (mitiga uso en tiempo real
  / competencia directa con los marketplaces). Alternativa descartada implícita:
  publicar en vivo.
- **Fair Market Price como percentiles P25/P50/P75** sobre ventana de 90 días
  con mínimo de muestras y exclusión de outliers, en lugar de media simple
  (más robusto ante listings anómalos; ver el max de 111.111 € en Hispasonic).
- **Licencia CC BY 4.0** (uso comercial permitido con atribución); `CITATION.cff`
  para citación académica.
- **Cadence de actualización semanal** mediante commits "Weekly update
  YYYY-MM-DD" (histórico en `origin/main`), pese a que el README dice
  "updated periodically".

## 5. Estado actual

- **Rama:** `main`. Única rama local y remota (`origin/main`). No hay ramas
  activas de trabajo.
- **Desincronización:** la copia local está **5 commits por detrás de
  `origin/main`** (fast-forward posible). Local está en `db57dc3` (release
  inicial, 2026-05); `origin/main` llega a `fb463e7` "Weekly update 2026-05-25"
  (332 fair prices, 3204 products, 15 monthly records). Falta `git pull`.
- **Datos locales (desactualizados):** fair_prices 122 modelos, products 3.037,
  monthly_market_stats hasta 2026-05 (incluye fuente Thomann/DE en 2026-03 y
  filas históricas sueltas de 2022–2023 de Hispasonic).
- **Cambios sin commitear:** un fichero sin seguimiento **`token_github.txt`** en
  la raíz que contiene un **GitHub Personal Access Token (`github_pat_...`) en
  claro**. No debe commitearse. Riesgo de seguridad: conviene **revocar ese
  token** y eliminar el fichero (o añadirlo a `.gitignore`). No aporta valor al
  dataset.
- **Sin `.gitignore`, sin CI, sin `DECISIONS.md` / `TODO.md` / `JOURNAL` /
  `docs/`.** Toda la documentación está en `README.md` y `CITATION.cff`.
- **Inconsistencias detectadas (PENDIENTE DE CONFIRMAR / corregir):**
  - README lista 4 marketplaces (Hispasonic ES, Soundsmarket ES, Audiofanzine
    EU, Noiz GR) pero `monthly_market_stats.csv` incluye también **Thomann (DE)**.
  - `source_country` de Audiofanzine aparece como **`US`** en el CSV; el README
    lo describe como marketplace europeo ("EU").
  - README dice `window_days` "(90 days)" como fijo pero es columna por fila.
  - Los recuentos del README (122 / 3.037) son los de la release inicial, no los
    de la cabeza remota.

## 6. Próximos pasos

Priorizados:

1. **Seguridad:** revocar el token de `token_github.txt`, borrar el fichero y
   crear `.gitignore` que lo excluya. Verificar que nunca entró en un commit
   (`git log --all -- token_github.txt`).
2. **Sincronizar:** `git pull` para alinear local con `origin/main` (5 commits).
3. **Aclarar el mapeo de `sources`** (IDs numéricos → nombre de marketplace) y
   publicarlo, idealmente como fichero `data/sources.csv` o sección en README.
4. **Resolver inconsistencias README ↔ datos:** añadir Thomann a la lista de
   fuentes o explicar por qué aparece; corregir `source_country` de Audiofanzine
   (`US`→`EU`/país real); actualizar los recuentos del README o expresarlos como
   "ver última release".
5. **Documentar la cadencia real** de actualización y, si procede, automatizarla
   (workflow/publisher que genere el commit "Weekly update").
6. **Confirmar y enlazar** dónde vive el pipeline de origen y su metodología
   detallada, para que este repo sea autocontenido en cuanto a procedencia.
7. Considerar añadir `DECISIONS.md` / `CHANGELOG.md` para no depender del
   mensaje de commit como único registro de cambios de dataset.
