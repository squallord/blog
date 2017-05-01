---
layout: news-article
title: The idea behind my mosaic gallery algorithm - Part I
meta: In this post I'll explain how it came to be. The general idea and the workarounds needed to make it work.
category: news
tag: programming
---

{{page.title}}
---

Tho see the functionality of this program, please see [**this**]({% for news in site.posts %} {% if news.title contains "How to use" and "mosaic gallery" %} {{site.baseurl}}{{news.url}} {% endif %} {% endfor %}){: style="text-decoration: none; color: darkblue"} before reading the article below.

This idea began with my girlfriend asking me if I could make a photo collage to celebrate our first dating anniversary. So, I began to look for some solutions for this problem (including existing programs that already solved this) and [**this article**](http://blog.vjeux.com/2012/image/image-layout-algorithm-lightbox-android.html){: style="text-decoration: none; color: darkblue"} caught my eye. The basic idea seemed pretty simple and I was already looking for some program to build using the Python language. However, I wanted to place the photos with as less restrictions as possible to make it feel more "organic".

Image manipulation is very expensive, so I thought I could build a simulator (kind of) using only numbers in a **txt** file. The process is pretty straightforward and we'll go through it step by step. So, let's begin with the very first foundation: the **image canvas**. The **image canvas** is an entity created to abstract the final image that will hold all photos together. This abstraction is achieved by representing each piece of a picture with a **cluster**. A **cluster** has a width and height in pixels and will depend both on the paperSize and the *clusterPow* attribute. A **chunk** is a collection of **clusters** that represents an image in this **image canvas** enviroment.

The canvas is built in a way that it has **minimal chunk pieces** times **minimal chunk pieces** both in **width** and **height**. A **minimal chunk piece** is evaluated by computing the expression **2^clusterPow** and it corresponds to the **smallest possible picture** in **width** and **height** **clusters units**.

"What the hell?!" you might be thinking now. Let's see an example. Imagine you've set **clusterPow** to **3**. That will produce a minimal chunk of **2^3** by **2^3** or **8** by **8** clusters in size. So, the paper will be split into **8 pieces** of **8 clusters** each in its **width** and **height**, giving us a total of 64 **clusters** in each dimension. The total number of **clusters** in this **image canvas** will be 64 x 64 = 4096 **clusters** (or 8 x 8 = 64 **minimal chunks**). Each picture can be associated with a certain **cluster**. The algoritm works with **4 sizes** of **clusters**, including the **minimal cluster**. There are **2 sizes** for **portrait** and **landscape** images. From the **smallest** to the **largest chunk** we have: (1, 1), (1, 2), (2, 2), (2, 4). The **chunk** (2, 4), for example, means that it's **2 minimal chunks** in **height** (**two rows** of **minimal chunk**) and **4 minimal chunks** in **width** (**four columns** of **minimal chunk**). In this example, the **minimal chunk** is **8** by **8 clusters** in size, so the **chunk** (2, 4) will be **16** by **32 clusters**. To sum up, we would have:

- clusterPow = 3;
- minimal chunk = 8 by 8 clusters;
- size of image canvas = 8 minimal chunks in both width and height, totalizing 64 minimal chunks;
- size of image canvas = 64 clusters in both width and height, totalizing 4096 clusters;
- chunk (1, 1) = 8 by 8 clusters -- the smallest portrait picture;
- chunk (1, 2) = 8 by 16 clusters -- the smallest landscape picture;
- chunk (2, 2) = 8 by 8 clusters -- the largest portrait picture;
- chunk (2, 4) = 8 by 8 clusters -- the largest landcape picture.

But how these **clusters** and **chunks** translate into image data? The answer is simple. In this example, we split the canvas in **8 pieces** (**minimal chunks**) in its **width** and **height**. If we set the **paperSize** parameter to **A3** = (3512, 4960), each cluster will translate to 3512/64 = **54.875** in **width** and 4960/64 = **77.5** in **height**. Strange, right? Indeed, you don't need to think about a single **cluster** in pixels because **clusters** are used only to check for **image intersection** (we'll see it in further detail) and no image will be represented by a single **cluster**. As you know, the smallest possible picture is **8** by **8 clusters** (the **minimal chunk**), which will result in (54.875 x 8) pixels by (77.5 x 8) pixels or **439** pixels by **620** pixels. The other **chunks** are just **linear combinations** of the **smallest chunk** attributes (**width** and **height**). No half pixels!

A blank 64 by 64 **image canvas** should look like this:

![blank canvas 64 x 64]({{site.baseurl}}/assets/mgallery/blank_canvas_64x64.png){: .center-image .drop-shadow-image}

As the **image canvas** is filled by pictures, the **clusters** numbers changes to match the **ID** of the picture. So, the first picture that the program places in this canvas will have the number **1** assigned to it in every **cluster** that represents the image, the second will have number **2** and so on.

Inside **mgallery folder**, there's a file called **log_highest.txt**. It contais the **highest rating completed image canvas** and is filled with pictures **IDs**.

![filled canvas 64 x 64]({{site.baseurl}}/assets/mgallery/filled_canvas_64x64.png){: .center-image .drop-shadow-image}

The picture **bellow** just shows how these numbers **translates** into images:

![mosaic photo gallery example A4]({{site.baseurl}}/assets/mgallery/mosaic_gallery_A4_4px.jpg){: .center-image .drop-shadow-image}

Let's take a closer look in this **log_highest.txt** file, just the **upper left** part, for example:

![filled canvas upper-left 64 x 64]({{site.baseurl}}/assets/mgallery/filled_canvas_64x64_upper-left.png){: .center-image .drop-shadow-image}

Let's take that beautiful landscape whose **ID** is **5**, for example. It fits the **largest landscape chunk**, so it takes **32 clusters** in **width** and **16 clusters** in **height**, as we expected. Remember that every **cluster** that has **5** assigned to it corresponds to the picture whose **ID** is **5**. If you still find confusing all these numbers mixed togheter, just take a look at the picture **bellow**, where I **highlighted** each sector (image chunk).

![filled canvas upper-left w/ highlighted 64 x 64]({{site.baseurl}}/assets/mgallery/filled_canvas_64x64_upper-left_highlighted.png){: .center-image .drop-shadow-image}

Hopefully, now you're getting a better understanding on how I've set up this "simulator" to fill the final piece with pictures. We'll continue to explore how this algorithm work in **Part II** of this article. Hope to see you there!