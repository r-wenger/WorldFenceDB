# GlobalFenceMap Dataset

## Overview
This dataset contains extracted fences from OpenStreetMap (OSM) for various regions worldwide. The dataset is structured by **region** (e.g., Asia, Caribbean, Europe), with each country or island having its own subfolder.

Each country folder contains different versions of the fence data at various processing stages.

---

## Folder Structure

Each **region** (e.g., Asia, Caribbean, Europe) contains subfolders for individual **countries**.  
Each country folder includes the following files:

- `<Country>.gpkg` → Raw extracted fences from OSM.
- `<Country>_classes.gpkg` → Fences where `fence_type` is not empty.
- `<Country>_cleaned.gpkg` → Standardized fences mapped to OSM's classification.
- `<Country>_classes_unique.txt` → List of unique `fence_type` values for manual mapping.

The **region** folder contains a csv mapping file:
- `all_fence_types.csv` → CSV mapping local `fence_type` values to standard OSM types.
  
---

## File Descriptions

1. **Raw Fences Data (`<Country>.gpkg`)**
   - Contains **raw** fence data extracted from OpenStreetMap.
   - Includes all fences regardless of whether the `fence_type` attribute is present.
   - Geometry: **LineString**.

2. **Filtered Fences (`<Country>_classes.gpkg`)**
   - Contains only fences where the **`fence_type`** attribute is **not empty**.
   - This file is a subset of the raw dataset, focusing on fences with a defined type.
   - The `fence_type` attribute follows OSM’s classification:
     - OSM Fence Type Key: https://wiki.openstreetmap.org/wiki/Key:fence_type

3. **Standardized Fences (`<Country>_cleaned.gpkg`)**
   - This version **remaps `fence_type` values** to standard OSM fence types for consistency.
   - Uses a predefined mapping file (`fence_type_mapping.csv`) stored in each country's folder.
   - Some ambiguous values are manually assigned to the closest OSM category.

4. **Unique Fence Types (`<Country>_classes_unique.txt`)**
   - Contains the **list of unique `fence_type` values** found in `<Country>_classes.gpkg`.
   - Used for mapping local values to standard OSM categories.

5. **Fence Type Mapping (`fence_type_mapping.csv`)**
   - A CSV file mapping country-specific `fence_type` values to the **OSM standard** classification.
   - Format:

     ```
      cleft_pale;wood
      cloture_animaliere;animal_control
      cloture_pour_animaux_(moutons);animal_control
      club_fun;NULL
     ```

---

## Data Processing Methodology

1. **Data Extraction**
   - The dataset was generated from **OpenStreetMap** using `.osm.pbf` files.
   - Extracted using **OGR/GDAL** with SQL filtering (`barrier='fence'`).

2. **Filtering (`_classes.gpkg`)**
   - Only fences with a **non-empty `fence_type`** were kept.

3. **Standardization (`_cleaned.gpkg`)**
   - `fence_type` values were matched to **OSM’s official fence classification**.
   - Non-standard or unclear values were mapped to `NULL` or the closest valid type.
   - The process ensures **global consistency** across all countries.

4. **Validation**
   - The list of **unique `fence_type` values** was saved per country (`_classes_unique.txt`).
   - Mappings were manually refined for edge cases.

---

## Notes & Limitations
- Some **custom/local fence types** might not directly fit the OSM standard.
- The **remapping process** is manually reviewed but may need adjustments for some regions.
- If you find **missing or incorrect mappings**, update `fence_type_mapping.csv` and rerun the standardization process.

---

## References
- **OpenStreetMap Barrier Tagging**:
  - OSM Barrier Wiki: https://wiki.openstreetmap.org/wiki/Key:barrier
  - OSM Fence Type Key: https://wiki.openstreetmap.org/wiki/Key:fence_type

---

## Contact
For any issues or improvements related to this dataset, please contact **Romain Wenger** at:  
📧 **romain.wenger@live-cnrs.unistra.fr**
