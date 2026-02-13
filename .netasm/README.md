`config-openmaptiles.json`

- change: include_ids: true
- delete: poi, poi_detail, housenumber, urban_areas, ice_shelf, glacier

`process-openmaptiles.lua`

- delete: housenumber, poi
- modify: "-- Remap coastlines" and remove "featurecla" stuff

```zsh
mkdir -p coastline && curl --time-cond coastline/water-polygons-wgs84.zip --output coastline/water-polygons-wgs84.zip --location https://osmdata.openstreetmap.de/download/water-polygons-split-4326.zip && unzip -oj coastline/water-polygons-wgs84.zip -d coastline
mkdir -p ".netasm/data~" && curl --time-cond ".netasm/data~/turkey.pbf" --output ".netasm/data~/turkey.pbf" --location https://download.geofabrik.de/europe/turkey-latest.osm.pbf
```

```zsh
docker build --tag=tilemaker .
docker run --rm -t --init --name tilemaker -v $(pwd)/.netasm/data~:/usr/src/app/data -v $(pwd)/coastline:/usr/src/app/coastline -v $(pwd)/resources:/usr/src/app/resources tilemaker --fast --input=data/turkey.pbf --output=data/turkey.mbtiles --config=resources/config-openmaptiles.json --process=resources/process-openmaptiles.lua
```
