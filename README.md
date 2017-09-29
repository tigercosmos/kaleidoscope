# ◄ ◢ ◣ ▲ Kaleidoscope ▼ ◄ ◢ ◣
Kaleidoscope is a tool to create kaleidoscope-like GIF arts using the [delaunay triangulation](https://en.wikipedia.org/wiki/Delaunay_triangulation) technique. It takes an image as input and it converts to abstract GIF composed of tiles of triangles.

### The technique
* First the image is blured out to smothen the sharp pixel edges. The more blured an image is the more diffused the generated output will be.
* Second the resulted image is converted to grayscale mode. 
* Then a [sobel](https://en.wikipedia.org/wiki/Sobel_operator) filter operator is applied on the grayscaled image to obtain the image edges. An optional threshold value is applied to filter out the representative pixels of the resulting image.
* Lastly we apply the delaunay algorithm on the pixels obtained from the previous step.

```go
blur = tri.Stackblur(img, uint32(width), uint32(height), uint32(*blurRadius))
gray = tri.Grayscale(blur)
sobel = tri.SobelFilter(gray, float64(*sobelThreshold))
points = tri.GetEdgePoints(sobel, *pointsThreshold, *maxPoints)

triangles = delaunay.Init(width, height).Insert(points).GetTriangles()
```
## Installation and usage
```bash
$ go get github.com/fogleman/gg
$ go build ./src/main.go
```
### Supported commands

```bash
$ ./main --help
```
The following flags are supported:

| Flag | Default | Description |
| --- | --- | --- |
| `in` | n/a | Input file |
| `out` | n/a | Output file |
| `blur` | 4 | Blur radius |
| `max` | 2500 | Maximum number of points |
| `noise` | 0 | Noise factor |
| `points` | 20 | Points threshold |
| `sobel` | 10 | Sobel filter threshold |
| `solid` | false | Solid line color |
| `wireframe` | 0 | Wireframe mode (without,with,both) |
| `width` | 1 | Wireframe line width |

Setting a lower points value, the resulted image will be more like a cubic painting. You can even add a noise factor, giving a more artistic, despeckle like result for the final image.  

Here are some examples you can experiment with:
```bash
$ ./main -in samples/clown_4.jpg -out output.png -wireframe=0 -max=3500 -width=2 -blur=2
$ ./main -in samples/clown_4.jpg -out output.png -wireframe=2 -max=5500 -width=1 -blur=10
```
## License
This project is based on [esimov/triangle](https://github.com/esimov/triangle), Which is under the MIT License. This project is also under MIT License.
