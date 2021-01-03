# Face Morphing
I used image warping to morph one or more images into another. I produced a "morph" animation of someone else's face into my face, computed the mean of a population of faces and extrapolated from a population mean to create a caricature of myself.  I transformed different images and create morphings between them. I learned how to use affine transformation, triangulation, and color interpolation to average, warp, and morph images.

## Defining Correspondences
#### Selecting the points:

#### The very first step was to define points that you will be “matching” in a sense across the two images you are trying to morph. This part is very tedious as you must pick the same number of points in the same order and locations across the two images. These points are the reference points for the rest of the entire morphing process because I use these points to triangulate, which is then used to average and compute a midway face.

#### The points that I marked are all prominent features of the face that are used as a mapping from one image to the next. For example, I'm attempting to match David Schwimmer's eyes to George Clooney's eyes.

#### Because this process was so tedious, I drew out a diagram as a reference while I picked the points to ensure that I was consistent across the two images (this is shown above on the right). I selected 58 points in each image and used the ginput function to select my correspondence points on each image. The image on the left above shows what appears while using the point selection ginput function.


## Creating the triangulation:


#### The next step is to use the selected points to create a “triangulation.”Wikipedia defines Delaunay triangulation to be, “for a given set P of discrete points in a plane is a triangulation DT(P) such that no point in P is inside the circumcircle of any triangle in DT(P).” I can see this in the image below as the triangles generated from the selected points are all empty (they contain no other triangle within it). I used the Delaunay triangulation function to compute the triangulation on each triangle.

#### In simpler terms, Delaunay triangulation is a triangular mesh that is applied over the selected ponts. The triangular mesh gives us triangle-to-triangle “matching” between the two images.

## Computing the "Mid-way Face":

#### Before creating the full morphing gif, I wanted to create the midway image to see what the blend of the two faces would look like. The first step is to compute the average of the two images by averaging the points in the two images. The second step is to warp both of the images to match that “average” image. For example, Ross’s chin is a little pointier than George’s. So, we want to warp Ross’s face to match the average, which will be flatter.

#### In order to warp the image to the average, we use the process of affine transformation. I use the inverse of the affine transformation matrix T that maps the source image to the midway image. This tells us which points in the source image we are warping and performs bilinear interpolation on those selected points in order to find the nearest pixel value (In my case, I simply rounded to the nearest integer). By using the inverse affine matrix from the source to the dest helps us because we don’t have to worry about the scenario where the image is blank. If you mapped from the source to a blank image, that could create a lot of holes since pixel values at locations that don’t land on any pixel coordinates can be obtained from the source image by rounding to the nearest pixel.

## Cross Dissolving:

#### The last step after warping the images is to create the midway face image by simply averaging the two warped images. All this requires is taking the average of the pixels in the two warped images. 

## The Morph Sequence:

#### Now I can create the dynamic gif of the morph from George's to Ross's face. Unlike the previous part, we want to create a weighted average of images and repeatedly warp the images to that weighted average.

#### weighted avg image = (1- t) * (George) + t * (Ross)

#### We change the value at which we are weighting the average at every time step (increasing from 0 to 1). This essentially means that when the morph starts with George's image, the parameter for George's image is 1 and the parameter for Ross's image is 0. However, as the morph continues, the parameter will increment slowly and finally end on Ross's face (where his parameter now equals 1 and George's equals 0).

#### I created 25 different midway images, each with a different warp factor. I compiled these images together into the morphing animations below. I also used this same increasing pattern for the cross-dissolve (the function that puts the images together).

## The "Mean face" of a population:
#### Using the Danes dataset of a population of people's faces, I computed an "average danish women's face". Using the points they selected for their project, I computed the average face using the code I previously wrote. (Essentially, all we are doing is averaging all the warped images of smiling females from this dataset). Below, I've shown each woman and their face after it's been warped to the average. After computing the average face, I combined each of the original images into the shape of the average face."

