# HDCA data portal — per-cell series store

Companion data repository of [hdca-data-portal](https://github.com/Hyunglok-Kim/hdca-data-portal), the web portal
for the HDCA product (daily soil moisture, vegetation optical depth and effective scattering albedo from SMAP
L-band brightness temperatures on the 36 km EASE-Grid 2.0, 2015–2025; Lee et al., *Remote Sensing of Environment*, accepted).

The portal reads these files by HTTP range request when a cell is clicked. They are **browse data, not the dataset**:
use the NetCDF files for any analysis (see the portal's Download tab).

```
series/HDCA_series_r<r0>-<r1>.pmtiles    one archive per band of grid rows r0..r1
```

Each archive is a PMTiles container used as a key-value store: the entry for grid cell (row, column) is addressed as
z = 10, x = column, y = row, and holds gzip of `uint16` little-endian `[3928 days][SM, VOD, OMEGA]`, where 65535 means
no retrieval and the values are SM × 1e-4 m³/m³, VOD × 1e-4, OMEGA × 2e-4. Day 0 is 2015-04-01. Only land cells with at
least one retrieval have an entry (105,938 cells).

Why a second repository: GitHub Pages serves one site of at most 1 GB, and the fields already take half of that.
Both sites are on the same origin, so no CORS is involved. Rebuilt by `pipeline/pack_series.py` in the portal repository.
