---
name: recipe-import
description: Convert recipes (text, photos of recipe cards/books, or web recipes) into a Kaana recipe-import JSON for Iasha's recipe bank. Trigger when she shares recipes and wants them added to Kaana — e.g. "import these recipes", "add these to my recipe bank", "put this recipe into kaana". For packaged-food nutrition LABELS use label-import instead.
---

# Kaana Recipe Import

Iasha shares one or more recipes (typed text, photos of recipe cards/cookbooks, or pasted web recipes). Produce ONE import file she pastes into Kaana (Import & Data → Import recipes).

## Output

Write `YYYY-MM-DD-recipe-import.json` (today's date) to her Downloads or wherever she asks, AND paste the JSON in chat.

```json
{
  "schema": "kaana-recipe-import/1",
  "recipes": [
    {
      "name": "Kandu Kukulhu (Tuna Curry)",
      "servings": 4,
      "servingSize": "1 bowl",
      "finishedWeight": null,
      "ingredients": [
        {"name": "tuna, raw", "qty": 500, "unit": "g"},
        {"name": "coconut milk", "qty": 200, "unit": "ml"},
        {"name": "onion", "qty": 1, "unit": "piece"},
        {"name": "curry leaves", "qty": 1, "unit": "sprig"}
      ],
      "method": ["Sauté the onion...", "Add the tuna and spices...", "Simmer in coconut milk 15 min."],
      "notes": "",
      "tags": {
        "diets": [], "allergens": ["fish","tree-nut"], "iddsi": 7,
        "lifestyle": ["halal"], "cuisine": ["maldivian"],
        "mealPeriods": ["lunch","dinner"]
      },
      "photo": null
    }
  ]
}
```

## Rules

1. **Ingredients**: plain names + qty + unit. Kaana matches them against IFCT/AFCD/AUSNUT itself — use simple food words ("chicken breast", "coconut milk", "red lentils"), not brands. Units: g, kg, ml, l, cup, tbsp, tsp, piece, slice, can, sprig, clove, pinch. Convert vague amounts to your best estimate.
2. **Method**: array of steps, one string each, imperative voice.
3. **Tags** (all optional but valuable — they power auto-fill):
   - diets: low-fodmap, gluten-free, low-sodium, diabetic, hehp, renal
   - allergens CONTAINED: milk, egg, peanut, tree-nut, fish, shellfish, soy, wheat, sesame (coconut counts as tree-nut; flag it in notes)
   - iddsi: 7, 6, 5 or 4; lifestyle: vegetarian, vegan, halal; cuisine: maldivian, south-asian, western, east-asian
   - mealPeriods it suits: breakfast, morning_snack, lunch, afternoon_snack, dinner, evening_snack
4. **Photo**: if she provided a food photo for the recipe, include `"photo": {"mime": "image/jpeg", "b64": "..."}` (any size — Kaana recompresses). Otherwise `null`. Never invent a photo.
5. Tags you assign are marked unverified in Kaana automatically — still, only tag what you're confident of and note uncertainties in `notes`.
6. Duplicate names are skipped on import — warn her if a recipe name already sounds like one she has.

## Close

Remind her: Kaana → Import & Data → Import recipes → paste → done; imported recipes appear in the Recipe Library flagged for review.
