---
title: Status of Local Administrative Units (LAUs) in Southern Europe
description: Interactive maps of LAUs in Portugal, Spain, Italy, and Greece, with methods and sources.
permalink: /
---

# LAU Maps

This site hosts country-level interactive maps for Portugal, Spain, Italy, and Greece that classify each municipality (LAU) by:
- <a href="https://ec.europa.eu/eurostat/web/gisco/geodata/population-distribution/degree-urbanisation"><strong>Degree of Urbanisation (DEGURBA)</strong></a> (urban–rural)
- <a href="https://ec.europa.eu/eurostat/web/gisco/geodata/geographic-accessibility"><strong>Service accessibility</strong></a> (hospitals and primary schools)
- <strong>Status</strong> (a summary of centrality and services)

Further info on methodological details and limitations can be found below.

## Interactive maps
- **Greece** — [Open map]({{ '/maps/GR_status.html' | relative_url }})
- **Italy** — [Open map]({{ '/maps/IT_status.html' | relative_url }})
- **Portugal** — [Open map]({{ '/maps/PT_status.html' | relative_url }})
- **Spain** — [Open map]({{ '/maps/ES_status.html' | relative_url }})

## Methodological Notes

<details open>
  <summary><strong>Datasets</strong></summary>
  <ul>
  <li><a href="https://gisco-services.ec.europa.eu/distribution/v1/lau-2021.html"><strong>LAU boundaries (2021): LAU_RG_01M_2021_3035.gpkg (ETRS89-LAEA)</strong></a></li>
  <li><a href="https://ec.europa.eu/eurostat/web/gisco/geodata/population-distribution/degree-urbanisation"><strong>DEGURBA at LAU (2021)</strong></a></li>
  <li><a href="https://ec.europa.eu/eurostat/web/gisco/geodata/population-distribution/population-grids"><strong>EU Census 1-km grid (2021)</strong></li>
  <li><a href="https://ec.europa.eu/eurostat/web/gisco/geodata/geographic-accessibility"><strong>Accessibility to services (2023)</strong></li>
  <li><a href="https://gisco-services.ec.europa.eu/distribution/v1/urau-2021.html"><strong>Functional Urban Areas (2021)</strong></li>
  </ul>
</details>

<details>
  <summary><strong>Data Wrangling and Processing</strong></summary>
  <p>The workflow is deliberately simple and EU-standard. I have started from Eurostat/GISCO’s 2021 LAU boundary file in ETRS89-LAEA and added the 2021 DEGURBA labels that classify each municipality as “Cities”, “Towns & suburbs”, or “Rural areas”. Since countries format LAU codes differently, I have normalised IDs by left-padding to each country’s modal length (Portugal and Italy: six characters; Spain: five; Greece: eight) before adding/merging them. 

Data on <strong>population</strong> comes from the 2021 EU 1-km census grid (layer <code>census2021</code>, using the total residents field) and is used to weight all accessibility calculations. Populated cells were assigned to municipalities with a centroid-within rule, which is fast and robust for large-area processing.

<strong>Service access</strong> was built from GISCO’s 2023 1-km travel-time indicators. For both hospitals and primary schools, I took the population-weighted average minutes to the three nearest providers (falling back to the nearest when needed). I then sum those two times into a single “access burden” and classify municipalities within each country: the best quarter are “Service-rich”, the worst quarter are “Service-poor”, and the remainder are “Average”. To avoid flattering outliers, there is a hard override: any municipality where more than one fifth of residents are over 30 minutes from a hospital or over 15 minutes from a primary school is marked Service-poor regardless of its quartile.

<strong>Centrality</strong> is anchored on the 2021 EU-OECD Functional Urban Areas. Municipalities whose centroids fall inside an FUA are split into “Core” if their DEGURBA label is Cities, otherwise “Commuting zone”. Municipalities outside any FUA are placed into distance bands based on how far their centroid lies from the nearest FUA boundary—“Near-FUA” within 30 km, “Intermediate” at 30–60 km, “Peripheral” at 60–120 km, and “Remote” beyond 120 km. The overall “status” combines these two dimensions in a way that matches the narrative I use in the thesis: “More central” if a municipality is in core/commuting/near-FUA and is not service-poor; “Left behind” if it is intermediate/peripheral/remote and is service-poor; “In-between” otherwise.
</p>
</details>

<details>
  <summary><strong>Data Visualization</strong></summary>
  <p>For the web maps I simplify geometries in ETRS89-LAEA and export them to WGS84 for Leaflet. The interactive views are built with Folium on top of CartoDB Positron tiles and expose the key fields in tooltips, including the underlying travel times and population used in the aggregation. Static figures for print are generated separately and use national north-up projections for readability.  </p>
</details>

<details>
  <summary><strong>Limitations</strong></summary>
  <p>There are limits worth keeping in mind. Spatial units are fixed at 2021, while the accessibility layer is from 2023, so any subsequent changes are not captured. Centroid assignments — both when (i) attaching grid cells to municipalities and when (ii) identifying municipality–FUA membership —can produce minor edge effects on borders and small islands (though they seem to behave well at national scale). The travel-time indicators are model-based and reflect the network and assumptions at the time of release - one needs to consider, then, that local realities may differ. Finally, the services classification is intentionally country-relative, so that the resulting bands are comparable across countries with different baseline access.</p>
</details>

## Sources

<details open>
<summary><strong>Materials</strong></summary>
<p>All source material is public and harmonised: Eurostat/GISCO LAU 2021 boundaries, Eurostat DEGURBA at LAU (2021), the EU 1-km population grid from the 2021 census, GISCO’s 2023 accessibility-to-services indicators for hospitals and primary schools, and the EU-OECD 2021 FUA layer.</p>
<summary><strong>Software</strong></summary>
<p>Python (Libraries: <code>GeoPandas</code>, <code>Pyogrio</code>, <code>Shapely</code>, <code>Pandas</code>, <code>Folium</code> and <code>Matplotlib</code>)</p>
</details>

<small>Last updated: {{ 'now' | date: '%Y-%m-%d' }}</small>
