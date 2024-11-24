<figure>
<img src="https://www.airport-technology.com/wp-content/uploads/sites/14/2024/11/2l-image-144.jpg" style="width: 1000px; height: 400px;">
<figcaption><h3>The Landsat9 satellite</h3></figcaption>
</figure> 
<h1>Basic Satellite Image Analysis</h1>
<p>In this repo we will look into basics of analzing satellite images. Satellite images are generally optimised with images having extension <code>.tiff</code> or images with multiple bands,<code>.tiff</code> is a tagged image file format , which is versatile and highly flexible image file format it supports high quality, lossless compression, multiple layers etc. A multiple band image is also known as a multi spectral image which captures data at different wavelenghts across the electromagnetic spectrum. Each band in the image corresponds to a specific range of wavelenghts.</p>
<p>Since it is a beginner level we do not use multiple band image since it will make the project a little complex. Here are the things we will look into in this repo</p>
<ol>
<li>RGB Bands</li>
<li>Normalization</li>
<li>visualization</li>
<li>RGB Composites</li>
<li>plotting the RGB composite</li>
<li>Applying Histogram Stretch</li>
<li>indices calculation</li>
  <ul>
      <li>VARI</li>
      <li>SAVI</li>
      <li>NDBI</li>
  </ul>
  <li>Plotting indices</li>
</ol>
<hr>
<h3>install necessary packages</h3>

```
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
```
<h3>Give the image path</h3>

```
image_path = "C:\\Users\\saeem\\Pictures\\Saved Pictures\\Screenshot 2024-11-24 141946.png"  # Update this path with your actual image file path
```
<h3>Open the image using PIL (pillow)</h3>

```
img = Image.open(image_path).convert("RGB")
```
<h3>Convert to NumPy array for processing</h3>

```
img_array = np.array(img)
```
<h3>Split into Red, Green, Blue channels</h3>

```
red = img_array[:, :, 0].astype(float)
green = img_array[:, :, 1].astype(float)
blue = img_array[:, :, 2].astype(float)
```
<h3>Normalize the bands</h3>

```
def normalize(array):
    # Adding a small epsilon to avoid division by zero
    epsilon = 1e-6
    return (array - array.min()) / (array.max() - array.min() + epsilon)

red = normalize(red)
green = normalize(green)
blue = normalize(blue)
````
<h3>Visualize individual bands</h3>

```
bands = [red, green, blue]
band_names = ["Red", "Green", "Blue"]

plt.figure(figsize=(15, 10))
for i, band in enumerate(bands, start=1):
    plt.subplot(1, 3, i)
    plt.imshow(band, cmap="gray")
    plt.title(f"{band_names[i-1]} Band")
    plt.axis("off")
plt.tight_layout()
plt.show()
```
<h3>Create RGB Composite</h3>

```
rgb_composite = np.dstack((red, green, blue))
```
<h3>Plot RGB Composite</h3>

```
plt.figure(figsize=(10, 10))
plt.imshow(rgb_composite)
plt.title("RGB Composite")
plt.axis("off")
plt.show()
```
<h3>Apply histogram stretch (normalization)</h3>

```
def stretch_histogram(array):
    epsilon = 1e-6  # To prevent divide by zero
    return (array - array.min()) / (array.max() - array.min() + epsilon)

stretched_rgb = np.dstack((
    stretch_histogram(red),
    stretch_histogram(green),
    stretch_histogram(blue),
))

plt.figure(figsize=(10, 10))
plt.imshow(stretched_rgb)
plt.title("Stretched RGB Composite")
plt.axis("off")
plt.show()
```
<h3>--- Indices Calculation ---</h3>
<h4># MNDWI (Water Index) = (Green - Blue) / (Green + Blue)</h4>
<h5>Adding small epsilon to avoid divide by zero</h5>

```
mndwi = (green - blue) / (green + blue + 1e-6)
```
<h4>VARI (Vegetation Index) = (Green - Red) / (Green + Red - Blue)</h4>

```
vari = (green - red) / (green + red - blue + 1e-6)
```
<h4>SAVI (Soil Adjusted Vegetation Index) = ((Green - Red) * (1 + L)) / (Green + Red + L)</h4>

```
L = 0.5  # Constant for SAVI
savi = ((green - red) * (1 + L)) / (green + red + L + 1e-6)
```
<h4>NDBI (Normalized Difference Built-up Index) = (Green - Red) / (Green + Red)</h4>

```
ndbi = (green - red) / (green + red + 1e-6)
```
<h3>--- Plot Indices ---</h3>

```
indices = [mndwi, vari, savi, ndbi]
index_names = ["MNDWI (Water Index)", "VARI (Vegetation Index)", "SAVI (Soil Adjusted Vegetation Index)", "NDBI (Geological Index)"]

plt.figure(figsize=(15, 10))
for i, index in enumerate(indices, start=1):
    plt.subplot(2, 2, i)
    plt.imshow(normalize(index), cmap="coolwarm" if "Index" in index_names[i-1] else "RdYlGn")
    plt.title(index_names[i-1])  # Fix reference to index_names
    plt.axis("off")
plt.tight_layout()
plt.show()
```

<p>This image was captured from "eos landviewer"</p>
<code>LOCATION:The Nilgiris,Nilgiri District, Tamil Nadu.</code><br>
<code>COORDINATES:11.39709°N 76.58521°E</code><br>
<code>AREA:25.00 km²</code><br>
<hr>
<h1>Conclusion</h1>
<p>Satellite image analysis helps in analyzing images taken by satellite, remote places wwhich are inaccessible by humans are easily analyzed using image analysis. It helps in determining the amount of vegetation in the area. It also helps analysing water bodies, geological factors etc. by plotting different indices. This project can be taken steps further by using high res satellite images or images with multiple bands this helps in giving more accurate details of the area</p>

<h1>Acknowledgement</h1>
<p> I would like to extend my deepest gratitude to Dr. Victor A.I, professor at Maharaja institure of technology Mysore, for his invaluable guidance and support throught the course of this projrct.</p>
<hr>

<b><h1><i>"KEEP EXPLORING ✌️"</i></h1></b>
