---
layout: news-article
title: A mosaic photo gallery algorithm
meta: How I made a program, using Python 2.7.x, to fit photos inside a canvas in a mosaic fashion.
category: news
tag: programming
---

{{page.title}}
---

Firstly, let's begin with the actual results. The code can be cloned (or downloaded) at <https://github.com/squallord/mgallery>{: style="text-decoration: none; color: darkblue"}. Feel free to use as you wish.

Inside mgallery folder there's this main python file called **mgallery.py** and a folder called **img_folder**. Inside **img_folder** you should put the images. From about 25 to 30 pics should do it. If you don't use enough pictures, then the program will end with an error message.

The main function which you should use to get the job done is **mosaic**. Before calling the main function, you need to call the python interpreter. Just follow the instructions bellow:

- Open the bash (ctrl + alt + t in most linux distros);
- Navigate to mgallery folder in your system;
- type in python;

Then something like this will show up:

{% highlight bash %}
$ python
Python 2.7.9 (default, Jun 29 2016, 13:08:31) 
[GCC 4.9.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
{% endhighlight %}

Before we go on I'd like to emphasize that this code is supposed to work just in linux and uses Python 2.7.x (newer versions problaby and/or other systems won't run it). Here's how you can make the function call:

{% highlight Python %}
>>> import mgallery
>>> mgallery.mosaic()
{% endhighlight %}

The mosaic function has some default parameters that will be discussed further. After typing the "*enter*" key, you should get something like the following output:

{% highlight bash %}
pixel chunks: [(1240, 1756), (1240, 878), (620, 878), (620, 439)]
29 pictures to form a mosaic gallery
ID: 19 with name download.jpg could not be resized
program at #1 attempt ...
> this canvas is completed! final attempt: 2 <
rate of images used: 82.14%
program at #2 attempt ...
> this canvas is completed! final attempt: 2 <
rate of images used: 92.86%
program at #3 attempt ...
> this canvas is completed! final attempt: 1 <
rate of images used: 82.14%
program at #4 attempt ...
> this canvas is completed! final attempt: 1 <
rate of images used: 71.43%
program at #5 attempt ...
> this canvas is NOT completed! <
rate of images used: 85.71%
program at #6 attempt ...
> this canvas is completed! final attempt: 2 <
rate of images used: 89.29%
program at #7 attempt ...
> this canvas is completed! final attempt: 2 <
rate of images used: 92.86%
program at #8 attempt ...
> this canvas is completed! final attempt: 2 <
rate of images used: 89.29%
program at #9 attempt ...
> this canvas is completed! final attempt: 2 <
rate of images used: 85.71%
program at #10 attempt ...
> this canvas is completed! final attempt: 1 <
rate of images used: 75.0%
... 9 completed image canvas
92.86 is the highest rating of this simulation
{% endhighlight %}

Then, you can navigate to your mgallery folder and find an image called **mosaic_gallery.jpg**. Jus open it an see the result. Pretty cool, right?

![mosaic photo gallery example]({{site.baseurl}}/assets/mosaic_gallery_4px_white.jpg){: .center-image .drop-shadow-image}

Next step is knowing how to customize this mosaic just a little. The funciton has six parameters. As there's some complexity involved in most of them, we'll see just two of them (padding and color) for now. The rest we'll leave untouched because it requires further knowledge of how the program works. Here is how the function was defined in terms of parameters:

{% highlight Python %}
def mosaic(folderName = "img_folder", \
	   maxAttempts = ct.MAX_ATTEMPTS, \
	   paperSize = ct.SZ_A3, \
	   clusterPow = ct.SZ_CLSTR, \
	   padding = ct.DEF_PADDING, \
	   color = ct.BCKGRND_CLR):
{% endhighlight %}

Where:
 
- folderName [string]: relative path to the folder that contains the images. Default value is "img_folder";
- maxAttempts [integer]: maximum number of times the algorithm tries to find a solution for the problem. Default value is 10;
- paperSize [tuple]: size of the paper in pixels. Default value is (3512, 4960);
- clusterPow [integer]: 2^clusterPow will define a 2^clusterPow x 2^clusterPow canvas to fit the pictures (this will be explained later on). Default value is 3;
- padding [integer]: padding in pixels to be added to each image. Default value is 2px;
- color [tuple]: the background color. Default value is (255, 255, 255).

Let's try to mess with that around a bit. Let's call the function with these parameters:

{% highlight bash %}
>>> mgallery.mosaic(padding = 10, color = (0, 0, 200))
{% endhighlight %}

And that will give us this picture:

![mosaic photo gallery example]({{site.baseurl}}/assets/mosaic_gallery_10px_blue.jpg){: .center-image .drop-shadow-image}

Notice that the borders of each picture are wider and have the blue **rgb**(0, 0, 200) tone to them. These are two of the most important parameters in this function. You can also change the paperSize to a smaller or higher value. But, you should keep the **aspect ratio** (width/height) at **1/1.42** (aproximately one over the square root of two). Other values can result in uneven scaling in some images (this might be improved later).

#### Published at {{page.date | date: "%B %d, %Y"}}.
{: style="text-align: right; font-weight: normal; margin: 0; padding: 0"}
#### Last modified at {{page.last_modified_at | date: "%B %d, %Y"}}.
{: style="text-align: right; font-weight: normal; margin: 0; padding: 0"}