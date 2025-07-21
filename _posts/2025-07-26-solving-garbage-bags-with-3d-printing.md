---
title: "Solving garbage bags with 3D printing"
excerpt_separator: "<!--more-->"
categories:
  - Making Things
tags:
  - Waste
  - 3D Printing
  - Making
  - Bins

header:
  image: /assets/images/bin-slicer-support.webp
  image_description: Part of a bin with support structure as displayed in 3d printer slicing software.
  caption: Support trees hugging a garbage bin
---

For many years, I've had a small, 10l organic waste bin standing on my kitchen counter. It looked nice, but it had some _very important_ luxury issues:

- I could not find any garbage bags that were small enough.
- The too-large, stiffer paper bags were hard to fit in.
- Where I live, you can't throw those "biodegradable" plastic bags into the organic waste, because they decompose too slowly.
- Those bags are not waterproof, so the bin was always dirty on the inside. I had to clean it all the time, and during that time I didn't have a bin for my organic waste.

My goal was to get rid of any garbage bags entirely, and to always have a bin available.

The solution: two bins. As soon as one is full, the other one takes over, until the first one is cleaned again.

Alas, I couldn't find any to buy.

<!--more-->

## DIY to the rescue.

I had three criteria for my new bins. They had to be:

- dishwasher-safe for easy cleaning
- stackable so as to take up the same space as the old single bin
- 3D printable in one piece

I [fired up onshape](https://cad.onshape.com/documents/b078e08ac22209f7f3720895/w/1556b4ab80866f37f1a044a5/e/ee8093f3356f4c7c60a361e3?renderMode=0&uiState=687e7ff234177930f182fb92) and got to drawing.

{% include image.html url="/assets/photos/bins.webp" alt="Onshape CAD drawing of the 2 bins and a lid" %}

## Stackable

The shape is really just a box with rounded corners and a lid on top. To make one bin fit into another, it had to have a narrow bottom and a wider top.

## Dishwasher-safe

For the material, I went with PETG instead of PLA (the cheapest, easiest, default 3D printing material), since PLA starts to become flexible at 55-60ºC, and PETG (should) at 80-86ºC - the so-called [glass transition temperature](https://all3dp.com/2/pla-petg-glass-transition-temperature-3d-printing/).

Well, either I have a particularly powerful dishwasher or I should have calculated with a higher margin:

{% include image.html url="/assets/photos/deformed-bin.webp" alt="Deformed bin after a visit at the dishwasher" caption="Deformed bin after a visit at the dishwasher" %}

Switching to [ASA](https://en.wikipedia.org/wiki/Acrylonitrile_styrene_acrylate) (100ºC) solved the problem. ASA is very close to ABS, and that's what lego bricks are made of, so it is solid.

## Printable

My original design featured a band around the top with a horizontal bottom edge - this is not something that can be 3D printed as is, because the edge would have to be floating in the air while being printed.

{% include image.html url="/assets/photos/bin-horizontal-edge.webp" caption="Bin with horizontal edge - to be floating in space." alt="CAD drawing of horizontal edge" %}

This meant that quite a substantial support structure was needed to print this - 90g of support material for 320g of material for the actual object.

{% include image.html url="/assets/photos/bin-slicer-support.webp" caption="Virtual view of the print plate with [tree supports](https://ultimaker.com/learn/tree-supports-what-are-they-and-how-do-they-work/) in green" alt="Virtual view of the print plate with tree supports in green." %}

The solution was to change the edge to a 45 degree angle, which can be printed without any support:

{% include image.html url="/assets/photos/bin-45deg-edge.webp" caption="45 degree edges can be printed" alt="CAD drawing with 45 degree edge" %}

## Leakage

One unexpected issue that came up with one of my countless (single digit number of) prototypes was that it was leaking liquids.

{% include image.html url="/assets/photos/bin-2-walls.webp" caption="2 outer walls plus infill" alt="View of the internal wall structure in the 3D printing slicer software" %}

In the default settings, walls are printed using a thin infill sandwiched between two layers of solid plastic.

{% include image.html url="/assets/photos/bin-solid-walls.webp" caption="Solid walls" alt="View of the solid wall in the 3D printing slicer software" %}

When I changed this to 6 walls, turning the walls into solid plastic, the problem was fixed.

## Conclusion

{% include image.html url="/assets/photos/bin.webp" caption="The finished products." alt="Photo of finished, nested bins in white with green lid." %}

After a few weeks of tinkering and printing, I can now live a slightly happier life, always a waste bin at my disposal, and no more plastic bags (at least for the organic waste).

And so can you - if you have access to a 3D printer. Here are the [print files](https://makerworld.com/en/models/1641967-nesting-bins#profileId-1735051). And the [original onshape CAD drawing](https://cad.onshape.com/documents/b078e08ac22209f7f3720895/w/1556b4ab80866f37f1a044a5/e/eda252627c090648c9170472?renderMode=0&uiState=6884fbcc0d7c1221b3f30783).

P.S. In a few years, I should have made up for all the printed plastic with the bags I now don't have to buy anymore. 😬
