# NutriSearch Label Extraction Instructions

Give Claude one or more photos of packaged-food nutrition labels and this document. Claude extracts the data and returns an import file for NutriSearch (Custom tab → "Import foods").

## Output format

Produce a single JSON file named `YYYY-MM-DD-label-import.json`:

```json
{
  "schema": "nutrisearch-food-import/1",
  "foods": [
    {
      "code": "PKG_anchor_lite_milk",
      "name": "Milk, lite, UHT (Anchor)",
      "cat": "Dairy",
      "db": "custom",
      "brand": "Anchor",
      "barcode": "9414742000000",
      "basis": "per100ml",
      "kcal": 47, "prot": 3.5, "fat": 1.5, "cho": 4.9, "fibre": 0, "sugar": 4.9,
      "ca": 125, "fe": 0, "zn": 0, "na": 45, "k": 0, "p": 0, "mg": 0, "iod": 0,
      "vita": 0, "vitd": 1.0, "vitc": 0, "fol": 0, "b12": 0.4, "b1": 0, "b2": 0, "nia": 0,
      "sfa": 1.0, "mufa": 0, "pufa": 0,
      "servingSize_g": 250,
      "servingLabel": "1 glass (250 ml)",
      "servingsPerPack": 4,
      "packSize": "1 L",
      "desc": "Anchor NZ | 1 L UHT | panel per 100 ml | kJ converted to kcal",
      "source": "label-photo",
      "dateAdded": "2026-07-03"
    }
  ]
}
```

## Rules

1. **All nutrient values are per 100 g (or per 100 ml for liquids — set `basis` accordingly).** If the label only shows per-serve values, divide by the serving size and multiply by 100.
2. **All 25 nutrient keys are required**: kcal, prot, fat, cho, fibre, sugar, ca, fe, zn, na, k, p, mg, iod, vita, vitd, vitc, fol, b12, b1, b2, nia, sfa, mufa, pufa. Nutrients not stated on the label = `0` (note which in `desc`).
3. **Units**: kcal; prot/fat/cho/fibre/sugar/sfa/mufa/pufa in g; ca/fe/zn/na/k/p/mg/vitc/b1/b2/nia in mg; iod/vita/vitd/fol/b12 in µg.
4. **Conversions**:
   - Energy in kJ only → kcal = kJ ÷ 4.184 (round to whole number).
   - Salt in g → sodium mg = salt g × 400 (i.e. salt ÷ 2.5 = sodium g).
   - Note every conversion applied in `desc`.
5. **Serving metadata** (this is why the workflow exists — always capture it):
   - `servingSize_g`: serving size in grams (or ml for liquids).
   - `servingLabel`: human-readable, e.g. `"2 biscuits (25 g)"`.
   - `servingsPerPack`: number of serves per pack, if stated.
   - `packSize`: net weight/volume, e.g. `"375 g"`, `"1 L"`.
   - If serving size or pack size is not visible in the photo, **ask** before finalizing.
6. **Naming**: `"Food, descriptor (Brand)"` style to match the database convention. `code` = `PKG_` + short slug.
7. **Provenance** in `desc`: brand/manufacturer | pack size | which panel basis was used | conversions applied | anything unreadable.
8. If the photo is unreadable or values look implausible (e.g. protein > 100 g/100 g), say so rather than guessing.

## After extraction

Tell the user: open NutriSearch → Custom tab → "Import foods" → paste the JSON (or choose the file) → Preview import → Import now. Duplicates are detected by code/name and can be skipped, replaced, or kept alongside.
