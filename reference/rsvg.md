# Render SVG into Bitmap

Render svg image into a high quality bitmap. When both `width` and
`height` are `NULL`, the output resolution matches that of the input.
When either `width` or `height` is specified, the image is scaled
proportionally. When both `width` and `height` are specified, the image
is stretched into the requested size.

## Usage

``` r
rsvg(svg, width = NULL, height = NULL, css = NULL)

rsvg_raw(svg, width = NULL, height = NULL, css = NULL)

rsvg_nativeraster(svg, width = NULL, height = NULL, css = NULL)

rsvg_webp(svg, file = NULL, width = NULL, height = NULL, css = NULL)

rsvg_png(svg, file = NULL, width = NULL, height = NULL, css = NULL)

rsvg_pdf(svg, file = NULL, width = NULL, height = NULL, css = NULL)

rsvg_svg(svg, file = NULL, width = NULL, height = NULL, css = NULL)

rsvg_ps(svg, file = NULL, width = NULL, height = NULL, css = NULL)

rsvg_eps(svg, file = NULL, width = NULL, height = NULL, css = NULL)
```

## Arguments

- svg:

  path/url to svg file or raw vector with svg data. Use
  [charToRaw](https://rdrr.io/r/base/rawConversion.html) to convert an
  SVG string into raw data.

- width:

  output width in pixels or `NULL` for default.

- height:

  output height in pixels or `NULL` for default

- css:

  path/url to external css file or raw vector with css data. This
  requires your system has a recent version of librsvg.

- file:

  path to output file or `NULL` to return content as raw vector

## Examples

``` r
# create some svg
options(example.ask=FALSE)
tmp <- tempfile()
svglite::svglite(tmp, width = 10, height = 7)
ggplot2::qplot(mpg, wt, data = mtcars, colour = factor(cyl))
#> Warning: `qplot()` was deprecated in ggplot2 3.4.0.
dev.off()
#> agg_record_6ba6e7264f9 
#>                      2 

# convert directly into a vector or bitmap graphics format
rsvg_pdf(tmp, "out.pdf")
rsvg_png(tmp, "out.png")
rsvg_svg(tmp, "out.svg")
rsvg_ps(tmp, "out.ps")
rsvg_eps(tmp, "out.eps")

# render into raw bitmap array
bitmap <- rsvg(tmp, height = 1440)
dim(bitmap) # h*w*c
#> [1] 1440 2057    4

# render to native raster object
nr <- rsvg_nativeraster(tmp)
# grid::grid.raster(nr)

# read in your package of choice
magick::image_read(bitmap)
#> # A tibble: 1 × 7
#>   format width height colorspace matte filesize density
#>   <chr>  <int>  <int> <chr>      <lgl>    <int> <chr>  
#> 1 PNG     2057   1440 sRGB       TRUE         0 72x72  
webp::write_webp(bitmap, "bitmap.webp", quality = 100)

# cleanup
unlink(c("out.*", "bitmap.webp"))
```
