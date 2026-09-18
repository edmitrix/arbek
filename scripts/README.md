# Scripts

## Convert product photos for the web

Originals live in `Photos/<category>/`. The site serves optimised JPGs from `public/images/products/<category>/`
named `<product-id>-1.jpg`, `<product-id>-2.jpg`, `<product-id>-3.jpg` (plus one `category.jpg` per category).

Requires Python with Pillow (`pip install pillow`). Edit the MAP, then run from the project root:

```python
import os
from PIL import Image
src, dst = "Photos/clothing", "public/images/products/clothing"
MAP = {
  "gold-dot-midi":    ["outfit 3.png", "outfit 3.1.png", "outfit 3.2.png"],
  # "<product-id>":   ["<shot 1>", "<shot 2>", "<shot 3>"],
}
os.makedirs(dst, exist_ok=True)
for pid, files in MAP.items():
    for i, f in enumerate(files, 1):
        im = Image.open(os.path.join(src, f)).convert("RGB")
        if im.height > 1600:
            im = im.resize((round(im.width * 1600 / im.height), 1600), Image.LANCZOS)
        im.save(os.path.join(dst, f"{pid}-{i}.jpg"), "JPEG", quality=82, optimize=True, progressive=True)
```

Then add the product to the `PRODUCTS` array in `index.html` with the same `id` and `cat`.
