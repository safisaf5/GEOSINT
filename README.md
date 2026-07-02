# 🗺️ GEOSINT — Analyse géographique OSINT

**GEOSINT** est un outil **OSINT de renseignement géographique** entièrement
client — une **page web autonome, en un seul fichier HTML**. À partir d'une
**localisation** (adresse, lieu, code postal, coordonnées GPS ou **clic sur la
carte**), il rassemble automatiquement les informations publiques disponibles et
produit une **fiche OSINT** sourcée, cartographiée et exportable.

> Aucun serveur, aucune base de données, aucune authentification, aucune
> installation : ouvrez `index.html` et tout tourne dans le navigateur.

---

## 🎚️ Modes de collecte

Un sélecteur **Portée de la collecte** permet de choisir les sources interrogées
lors de la génération (le choix est mémorisé dans le navigateur) :

| Mode | Ce qui est collecté |
|---|---|
| 🛰️ **Satellite + carte** | Imagerie satellite (Esri, **Esri Wayback** datée, **Yandex**) **+** cartes (OpenTopoMap) et liens externes. Sans collecte OSM — rapide. |
| 🌐 **Tout** | Complet : imagerie + cartes **+** données OpenStreetMap/Overpass (réseau routier & ferré, transports, POI, environnement, **accès** dérivé, relief, villes) **+** Wikipédia à proximité et **photos géolocalisées** (Wikimedia Commons). |

L'interface masque automatiquement les cartes non pertinentes selon le mode
choisi.

## ✨ Fonctionnalités

- **Recherche multi-modes** : adresse / lieu / code postal (géocodage
  Nominatim), coordonnées GPS directes, ou **clic sur la carte interactive**.
- **Cartographie Leaflet** avec plusieurs fonds :
  - OpenStreetMap
  - OpenTopoMap (relief)
  - Imagerie satellite **Esri World Imagery**
  - Tuiles **Yandex** satellite
- **Conversions de coordonnées** complètes : décimal, **DMS**, **UTM**,
  **MGRS**, **Plus Code**, **GeoHash**.
- **Environnement & accessibilité** via Overpass/OSM : distance à l'**autoroute**,
  aux routes principales, à la **gare** et à l'**aéroport** les plus proches.
- **Météo & climat** en temps réel (**Open-Meteo**).
- **Liens externes OSINT** générés automatiquement vers les principaux outils de
  vérification et d'imagerie : Google Maps / Earth, Bing, Apple Plans, Yandex,
  Mapillary, KartaView, Wikimapia, what3words, SunCalc, Windy, Geoportail,
  Géoportail Suisse (map.geo.admin.ch), FlightRadar24, MarineTraffic,
  NASA Worldview, Sentinel Hub, Wikipédia/Wikimedia Commons…
- **Exports** : fiche OSINT mise en forme, **GeoJSON** et données brutes.
- **Images téléchargées au format WebP** (qualité 0,85) — plans satellite,
  captures Wayback/Yandex et images annotées, nettement plus légers que le PNG.
- **Interface 100 % en français**.

## 🚀 Utilisation

### En local

Aucune dépendance à installer — la page ne charge que la bibliothèque **Leaflet**
depuis un CDN.

```bash
# Option 1 : ouvrir directement le fichier
open index.html          # macOS

# Option 2 (recommandée) : servir en local pour éviter certains blocages CORS
python3 -m http.server 8000
# puis ouvrir http://localhost:8000
```

> ⚠️ La page appelle des APIs publiques (Nominatim, Overpass, Open-Meteo) et
> charge des tuiles distantes. Servez-la de préférence en **HTTPS** en
> production pour éviter les blocages *mixed-content* / CORS.

### Déploiement

C'est un site **statique** : déployable tel quel sur GitHub Pages, Netlify,
Vercel, Cloudflare Pages ou tout hébergement statique. Il suffit de servir
`index.html`.

## 🔌 Sources de données & services utilisés

| Service | Rôle |
|---|---|
| **Nominatim** (OpenStreetMap) | Géocodage / géocodage inverse |
| **Overpass API** | Objets OSM à proximité (routes, gares, aéroports, POI) |
| **Open-Meteo** | Météo & climat |
| **Esri World Imagery** | Imagerie satellite |
| **OpenTopoMap** | Fond topographique / relief |
| **Yandex** | Tuiles satellite complémentaires |
| **Leaflet** (CDN unpkg) | Moteur de carte interactive |

## 📜 Attributions & licences des données

- OpenStreetMap / Nominatim / Overpass — **ODbL 1.0**, © OpenStreetMap contributors.
- OpenTopoMap — **CC-BY-SA**.
- Esri World Imagery — © Esri, Maxar, Earthstar Geographics (attribution requise).
- Open-Meteo — **CC BY 4.0**.

Merci de respecter les **politiques d'usage** (fréquence d'appel) de Nominatim et
d'Overpass. GEOSINT n'agrège que des informations **publiques** et affiche pour
chaque bloc sa source : une donnée sans source n'est jamais présentée comme un
fait.

## ⚖️ Cadre d'usage

Outil destiné à des usages **légitimes** : recherche ouverte (OSINT),
vérification journalistique, analyse cartographique, éducation. L'utilisateur est
responsable du respect des lois applicables et des conditions d'utilisation des
services tiers interrogés.

## 📁 Structure

```
index.html      Application complète (HTML + CSS + JS, un seul fichier autonome)
README.md       Ce document
LICENSE         Licence MIT
```

## 👤 Auteur

**safisaf5**

## 📄 Licence

Distribué sous licence **MIT** — © 2026 safisaf5. Voir [LICENSE](LICENSE).
