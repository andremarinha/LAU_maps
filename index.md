---
title: Status of Local Administrative Units (LAUs) in Southern Europe
description: Interactive maps of LAUs in Portugal, Spain, Italy, and Greece, with methods and sources.
permalink: /
---

# LAU Maps

This site hosts country-level interactive maps of LAUs in Greece, Italy, Portugal, and Spain as a transparent companion to my PhD thesis.

Further info on methodological details and limitations can be found below.

## Interactive maps
- **Greece** — [Open map]({{ '/maps/greece_postoffices_map.html' | relative_url }})
- **Italy** — [Open map]({{ '/maps/italy_postoffices_map.html' | relative_url }})
- **Portugal** — [Open map]({{ '/maps/portugal_postoffices_map.html' | relative_url }})
- **Spain** — [Open map]({{ '/maps/spain_postoffices_map.html' | relative_url }})

## Methodological Notes

<details>
  <summary><strong>Data Collection</strong></summary>
  <p>The spatial data on post offices were obtained from OpenStreetMap (OSM), an openly licensed, community-curated geographic database. Specifically, I used the Overpass API, which allows structured queries against OSM. For each of the countries under study — Greece, Italy, Portugal, and Spain — I retrieved all features tagged with <code>amenity=post_office</code> tag, which is the standard OSM classification for physical post office facilities and captures both stand-alone buildings and smaller service points. By relying on OSM rather than official operator registries, I ensured a harmonized, reproducible dataset across multiple countries, accessible without the administrative or licensing constraints of national postal operators.</p>
</details>

<details>
  <summary><strong>Data Wrangling and Processing</strong></summary>
  <p>The raw OSM data contained three types of geometries: points (nodes), polygons (ways), and multipolygons (relations). To create a consistent point-based dataset, all features were converted to single centroid coordinates (using the out center directive in Overpass for non-point geometries). The resulting dataset included geographic coordinates, OSM IDs, and available metadata such as postal codes, operators, or opening hours. These data were saved in both <strong>GeoJSON</strong> and <strong>GeoPackage</strong> formats for transparency and reusability. For visualization, geometries were projected to EPSG:3857 (Web Mercator), the standard for web mapping, while storage and analysis were conducted in EPSG:4326 (WGS84). Data cleaning steps involved checking for duplicates, validating coordinate ranges, and discarding features without spatial information. A light quality assurance step was also performed by comparing point density in selected urban and rural areas with known post office distributions, confirming broad consistency.</p>
</details>

<details>
  <summary><strong>Data Visualization</strong></summary>
  <p>In the context of this (small) project, two forms of data visualization were produced. First, the interactive web maps that can be consulted above were created using <strong>Folium</strong>, which leverages Leaflet.js and CartoDB Positron basemaps. These maps allow exploration of individual features through marker popups and were used for inspection and qualitative assessment. Second, high-resolution static maps were generated with GeoPandas, Matplotlib, and Contextily. These outputs were specifically tailored for further inclusion in the body of dissertation, with markers being rendered at a uniform small size to highlight density and distribution, while basemaps were included at moderate zoom levels to provide geographic context.</p>
</details>

<details>
  <summary><strong>Limitations</strong></summary>
  <p>Taking the OSM route/approach has several inherent limitations that need to be considered. First, OSM is <strong>crowdsourced</strong>, which means coverage may not be uniform — metropolitan areas tend to be well mapped, while rural areas may contain gaps or outdated entries. Second, the reliance on the <code>amenity=post_office</code> tag can produce discrepancies when compared with official registries. For example, in Portugal, OSM identified <strong>985 post offices</strong>, whereas a manual coding exercise based on the official <a href="https://tinyurl.com/36f7v6jz">CTT website</a> yielded only <strong>564 branches</strong>. Part of this divergence likely arises from the inclusion in OSM of <strong>“pontos CTT”</strong> (partner outlets hosted in shops or other businesses), which provide limited services (i.e., mainly the sending and receiving of letters and parcels) rather than the full suite of services available in official <strong>“balcões”</strong> (CTT branches). Third, the dataset reflects OSM at the date of collection and does not capture subsequent openings, closures, or relocations. Finally, although national operators (Correos, Poste Italiane, CTT Portugal, Hellenic Post) maintain their own lists, these are not always openly accessible in harmonized formats. Despite these caveats, OSM offers a consistent, transparent, and reproducible dataset across multiple countries, making it suitable for comparative geographic analysis, provided the results are interpreted with these differences in mind.</p>
</details>

## Sources

<details open>
<summary><strong>Basemap/tiles</strong></summary>
<p>© OpenStreetMap contributors (see <a href="https://www.openstreetmap.org/copyright">copyright &amp; attribution</a>).</p>
</details>

<details>
<summary><strong>Country datasets</strong></summary>
<ul>
  <li><strong>Portugal:</strong> <!-- Add source + URL --></li>
  <li><strong>Spain:</strong> <!-- Add source + URL --></li>
  <li><strong>Italy:</strong> <!-- Add source + URL --></li>
  <li><strong>Greece:</strong> <!-- Add source + URL --></li>
</ul>
</details>

## How to cite

> André Marinha ({{ 'now' | date: '%Y' }}). “Post office locations — &lt;country&gt; (interactive map).”  
> <code>{{ site.url }}{{ site.baseurl }}/maps/&lt;country&gt;_postoffices_map.html</code> (accessed {{ 'now' | date: '%Y-%m-%d' }}).

<small>Last updated: {{ 'now' | date: '%Y-%m-%d' }}</small>
