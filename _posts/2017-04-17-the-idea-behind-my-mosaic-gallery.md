---
layout: news-article
title: The idea behind my mosaic gallery algorithm
meta: In this post I'll explain how it came to be. The general idea and the workarounds needed to make it work.
category: news
tag: 3dart
---

Tho see the functionality of this program, please see **this** before reading the article below.

This idea began with my girlfriend asking to me if I could make photo collage to celebrate our first dating anniversary. So, I began to look for some solutions for this problem (including existing programs that already solved this) and this article caught my eye: <http://blog.vjeux.com/2012/image/image-layout-algorithm-lightbox-android.html>{: style="text-decoration: none; color: darkblue"}. The basic idea seemed pretty simple and I was already looking for some program to make using the Python language. However, I wanted to place the photos with as less restrictions as possible to make it feel more "organic".

Image manipulation is very expensive, so I thought I could build a simulator (kind of) using only numbers in a txt file. The process is pretty streightforward and we'll go though it step by step. So, let's begin with the very first foundation: the **image canvas**. The **image canvas** is an entity created do abstract the final image that will hold all photos together. This abstraction is achieved representing each piece of a picture with a **cluster**. A **cluster** represents a width and height in pixels and will depend both on the paperSize and the clusterPow attribute. A **chunk** is a collection of clusters that represents an image in this **image canvas** enviroment.

The paper will be split into ((2^clusterPow)^2)^2 clusters. The *minimal chunk* size is 2^clusterPow cluster units which will give us the dimensions of the image canvas which will be *mininal chunk size* x *minimal chunk size* in cluster units, or (*minimal chunk size*)^2. So, the number of paper pieces would be given by (*minimal chunk size*). This *minimal chunk size* is the size of the smallest possible picture that could be placed on the **image canvas**. 

"What the hell?!" you might be thinking now. Let's see an example. Imagine you've set *clusterPow* to 3. That will produce a minimal chunk of 2^3 by 2^3 or 8 by 8 clusters in size. So, the paper will be split into 8 pieces of 8 clusters in its width and height, giving us a total of 64 clusters in each dimension. The total number of **clusters** in this **image canvas** will be 64 x 64 = 4096. Each picture can be associated with a certain cluster. The algoritm works with 4 sizes of clusters, including the minimal cluster. There are 2 sizes for portrait and landscape images. From the smallest to the largest chunk we have: (1, 1), (1, 2), (2, 2), (2, 4). The chunk (2, 4), for example, means that it's 2 minimal chunks in height (tho rows of minimal chunk) and 4 minimal chunks in width (four columns of minimal chunk). In our previous example, the minimal chunk is 8 by 8 clusters in size, so the chunk (2, 4) will be 16 by 32 clusters. To sum up, we would have:

- minimal chunk = 8 by 8 clusters;
- size of image canvas = 8 minimal clusters in both width and height;
- size of image canvas = 8 x 8 = 64 clusters in both width and height;
- chunk (1, 1) = 8 by 8 clusters -- the smallest portrait picture;
- chunk (1, 2) = 8 by 16 clusters -- the smallest landscape picture;
- chunk (2, 2) = 8 by 8 clusters -- the largest portrait picture;
- chunk (2, 4) = 8 by 8 clusters -- the largest landcape picture.

But how these clusters and chunks translate into image data? The answer is simple. In this example, we split the canvas in 8 pieces (minimal chunks) in its width and height. If we set the paperSize variable to A3 = (3512, 4960), each cluster will translate to 3512/64 = 54.875 in width and 4960/64 = 77.5 in height. Strange, right? Indeed, you don't need to think about a single cluster in pixels because clusters are used only to check for image intersection (we'll see it in further detail) and no image will be represented by a single cluster. The smallest image will be 439 x 620 in pixel units. No half pixels!

A blank 64 by 64 image canvas will look like this:

![blank canvas 64 x 64]({{site.baseurl}}/assets/mgallery/blank_canvas_64x64.png){: .center-image .drop-shadow-image}

As the image canvas is filled by pictures, the cluster numbers changes to match the ID of the picture. So, the first picture that the program places in this canvas will have the number 1 assigned in every cluster that represents the image.