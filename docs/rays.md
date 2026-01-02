# Rays, Simple Camera and Background

## The Ray

One thing that all ray tracers have is a ray class and a computation of what color is seen along a ray
To represent ray as a function we can go with this expression

P(t) = A +tB

Here,
P is a 3D position along line in 3D
A is the ray origin
B is the ray direction
and t is the coeffient of B (t is a real number)

![Ray](../assets/ray.png)

This is the interpretation of the ray

The code for this implementation is in headers/ray.h file

## Sending Rays to the Screen

A ray tracer sends rays through pixels and computes the color seen in the direction of those rays

The involved steps are

1. Calculate the ray from the “eye” through the pixel,
2. Determine which objects the ray intersects, and
3. Compute a color for the closest intersection point.

We will use 16:9 aspect ratio images to determine the height of the screen

Now we will calculate the viewport width using this code

```c++
auto aspect_ratio = 16.0 / 9.0;
int image_width = 400;

// Calculate the image height, and ensure that it's at least 1.
int image_height = int(image_width / aspect_ratio);
image_height = (image_height < 1) ? 1 : image_height;

// Viewport widths less than one are ok since they are real valued.
auto viewport_height = 2.0;
auto viewport_width = viewport_height * (double(image_width)/image_height);
```

\*Note: make sure to have the image height > 1

We are not using the aspect ration directly to calculate the viewport width because it is an ideal ratio. It may not be the actual ratio between the image_width and image_height

## Camera

Now we will define the camera center, a point in the 3D space from where all the rays will originate (it is commonly refered as the eye point)
The vector from the camera center to the viewport center will be orthogonal to the viewport (90 degree)

For the axis,
we will have the y-axis go up, the x-axis go right and the negative z-axis pointing to the viewing direction (also known as right-hand coordinates)

Let's start at the upper left pixel (pixel, 0, 0) scan left-to-right across each row, and then scan row-by-row, top-to-bottom

To navigate from the pixel grid,
We will use a vector (Vu) from left to right and another vector (Vv) from top to bottom.
Also the deltas will be calculate as 𝚫𝐮 and 𝚫𝐯

## Background

For the background, we will fill in the ray_color function to implement a simple gradient

We will use this formula to blend the colors

```
blendedValue=(1−scale) * startValue + scale * endValue
```
