---
name: How does a computer calculate a color gradient?
tools: []
image: assets/gradient.png
description: For a project that I'm working on, I'd like to add a color gradient to align with the stylistic choices of the product.
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Calculating a Color Gradient

> Okay, but *how* blue is the sky?

For a project that I'm working on, I'd like to add a color gradient to align with the stylistic choices of the product. Upon implementation however, I'm having trouble writing a function to generate the Hexadecimal values for colors in the color gradient.

> **Goal**: Write a function which, given two Hexadecimal codes can generate a series of interim steps between them

I realized that fundimentally, I don't understand the way we represent colors digitally. Let's do some research and write this function!

---

## What Does Hexadecimal Represent?

It's pretty widely known that computers use a combination of red, green and blue to show color on a screen. If you've ever heard RGB, this is what it refers to. A simple explanation as to why: these are the colors that the human eye can see. Though vastly oversimplifying, it's easiest to imagine that your eye has 3 different types of cells - cone cells - which are responsible for seeing red, green and blue respectively. All colors that we see are then combination of these base three and can be derived from different intensities of red, green and blue, respectively.

> Note: [“Cones respond to both the wavelength and intensity of light … Cones cannot detect color by themselves; rather, color vision requires comparison of the signal across different cone types”](https://en.wikipedia.org/wiki/Photoreceptor_cell#:~:text=A%20photoreceptor%20cell%20is%20a,that%20can%20stimulate%20biological%20processes)

The human eye is where the RGB color system comes from. Monitors use this system to encode color into tiny red, blue and green LEDs. The format that we store this information for each pixel is `(R,G,B)` where each position is a number from 0-255 representing at what intensity/brightness each color will be shown at.

As it turns out, [Hexadecimal is actually the same thing](https://en.wikipedia.org/wiki/Hexadecimal#:~:text=Hexadecimal%20numerals%20are%20widely%20used,which%20is%20half%20a%20byte). The format for Hexadecimal is `#RRGGBB` where the 0-255 values are represented by numbers 0-9 and letters A-F which translate to be a range of `0-16` when combined. Hexadecimal is a [base 16 number system](https://www.bbc.co.uk/bitesize/guides/zp73wmn/revision/1), as opposed to our standard base 10 system. Since `16*16=256` we can see how this is a direct translation, after making room for a 0.

> For example: `#4FCC79` = `(79, 204, 121)`

<center> 4F = 79, CC = 204, 79 = 121 </center>

Denary	     | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15
Hexadecimal	 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | A  | B  | C  | D  | E  | F

---

## Visualizing RGB as a 3D Space

So, we have red, green and blue on linear scales from 0 to 255. What does this mean? This means that we can represent the colors in three-dimensional space. Don't worry, I'll draw it. But first, let's understand our axis. We have three, Red, Green, and Blue. The sides of our three dimensional space will look like this:
<br>
![Three Axis](../assets/blog/gradient/Axis.png)
<br>
Combining them into a full cube (connecting them at 0) this is what we get:

![Cube](../assets/blog/gradient/Color%20Wheel.png)
<center><i>Note: I changed out #000000 (black) for #FFFFFF (white) since it looks nicer!</i></center>

This makes it a little bit easier to imagine the math that we're going to be doing to calculate a gradient. Imagine that inside this cube, we select two points, now physical locations in the cube, to calculate a gradient between. The literal representation of this gradient would be a straight line between the points.

---

## Calculating the Gradient

Let's start by choosing two random points, #264653 and #2A9D8F. Our finished gradient should look something like this:
<br>
![sample points gradient](../assets/blog/gradient/Gradient-Btw-Sample-Points.png)
<br>
A line between two points in 3D space is defined by: $r(t) = r_0 + tv$ where:
- $$r_0$$: an initial point=$$(x_0, y_0, z_0)$$
- $$v$$: direction (slope) vector = $$<x-x_0, y-y_0, z-z_0>$$

Using this knowledge we can calculate the slope of the line between the two points
$$v=<42-38, 157-70, 143-83>=<4,87,60>$$ and so:
<br>
<center> $$r(t)=(38,70,83)+t<4,87,60>$$ where $$0\leq t\leq1$$ </center>
Some sample output points from this equation are:
- 264E59
- 26575F
- 276065
- 27686B
- 287171
- 287A77
- 28827D
- 298B83
<br>

Which looks like this:

![sample points](../assets/blog/gradient/Calculated-Grad-Btw-Sample-Points.png)

We've done it! To adjust the space between the interim steps we can multiply the slope by $$1/(x+2)$$ where x is how many steps we'd like.