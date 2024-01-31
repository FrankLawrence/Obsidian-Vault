Y = 542
X = 402

Width = 744
Halfw = 372 = a.BoundingBox(3)/2

Height = 78
Halfh = 39 = a.BoundingBox(4)/2

max = 0
a = stats(1)
X = ceil(a.Centroid(1))
Y = ceil(a.Centroid(2))
Halfw = a.BoundingBox(3)/2
Halfh = a.BoundingBox(4)/2
imshow(color(Y-Halfh : Y+Halfh, X-Halfw : X+Halfw, 2))