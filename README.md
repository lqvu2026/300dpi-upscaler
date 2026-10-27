# 300 DPI Upscaler

A single-page web app that enlarges an image to the pixel size a 300 DPI print needs, then saves it as a PNG or JPEG with 300 DPI written into the file. It runs entirely in the browser. No image is uploaded anywhere.

## Use it

1. **Choose an image.** Click the drop area or drag a JPG, PNG, WebP or GIF onto it.
2. **Set the size.**
   - *Print size:* pick a paper size (4 × 6 in up to A3) or enter a custom print width in inches or cm. The image is fitted inside the paper size without cropping.
   - *Enlargement:* pick 2×, 3× or 4×.
3. **Portrait and soften (optional).**
   - *Portrait:* tick the box to blur the background. A dashed oval appears on the Original preview and marks the area that stays sharp. Use **Focus size**, **Focus across**, **Focus down** and **Edge softness** to fit the oval to your subject, and **Background blur** to set how strong the blur is.
   - *Skin smoothing:* evens out low-contrast texture such as skin while keeping strong edges (eyes, lips, hair) sharp.
   - *Soft focus glow:* adds a gentle glow over the whole picture.
4. **Finish and export.** Set the sharpening amount and the file type (PNG, or JPEG with a quality slider), then press **Upscale to 300 DPI**. Press **Save file** when the preview appears.

After the first run, changing an effect, sharpening or the file type and pressing the button again is quick, because the enlarged image is kept in memory. Changing the print size or the image starts the enlargement again.

The readout under Step 2 shows the new pixel size, the print size in inches and cm, and the enlargement factor before you run anything.

## Run it

Open `index.html` in a browser. No build step, no dependencies.

To host it on GitHub Pages: push `index.html` to a repository, then go to **Settings → Pages**, choose the branch and the root folder, and save. The page loads its fonts from Google Fonts, and falls back to system fonts if they are unavailable.

## How it works

- **Size:** target pixels = print size in inches × 300.
- **Resampling:** large enlargements are done in steps of up to 2× with the browser's high-quality smoothing, which avoids blocky results.
- **Portrait:** the background is blurred by shrinking and re-enlarging the image in halves, then the original is laid back over it through a soft oval mask.
- **Skin smoothing:** each pixel is blended toward a lightly blurred copy, and the blend fades out wherever the difference between the two is large, so edges stay crisp.
- **Soft focus:** a wider blurred copy is mixed over the image, with a little screen blending for the glow.
- **Sharpening:** an optional unsharp mask restores edge contrast lost to smoothing. It runs last, after the effects.
- **300 DPI tag:** a `pHYs` chunk is added to PNG files (11,811 pixels per metre) and the JFIF density fields are set in JPEG files. Print software and image editors read these as 300 DPI.

## Limits

- Enlarging adds pixels, not detail. A sharp, well-exposed original gives the best print, and enlargements above about 4× will look soft.
- Output is capped at 16,000 pixels per side and about 120 megapixels. Phones and older browsers may run out of memory sooner. Choose a smaller print size if you see an error.
- Portrait uses an oval focus area that you place yourself. It does not detect people, so the oval may include or miss parts of the background.
- Effects need extra memory. Above about 40 megapixels the enlarged image is not kept between runs, so each run starts the enlargement again.
- Transparent PNGs are flattened onto white when saved as JPEG.
- Browsers cannot always open HEIC photos. Convert them to JPG first.
