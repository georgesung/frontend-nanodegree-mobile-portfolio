## Website Performance Optimization portfolio project

This is a page speed optimization project, part of Udacity's Front-End Developer Nanodegree program. For the original README and source files provided by Udacity, please refer to the GitHub repo from which this repo was forked.

Below is the documentation for parts 1 and 2 of this project.

#### Part 1: Optimize PageSpeed Insights score for index.html

In this part of the project, I optimized index.html. For the original source code and resource files, please refer to the original Udacity project repo.

##### How to run this application

1. Check out the repository
1. To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
1. Download and install [ngrok](https://ngrok.com/) to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ngrok http 8080
  ```

1. Copy the public URL ngrok gives you and try running it through PageSpeed Insights! Optional: [More on integrating ngrok, Grunt and PageSpeed.](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)

##### Optimizations that I made

**Note:** I minified index.html. For reference, I saved the un-minified version at index_full.html

* Resized views/images/pizzeria.jpg such that the image width is 100px
  * In index.html, this images width is hard-coded to display at 100px width, so no need to download the original image with size 2048 x 1536
  * Image went from 2.25 MB to 22.8 KB, 99x size reduction
* Per PageSpeed Insight's recommendation, executed lossless image compression on images/pizzeria.jpg and img/profilepic.jpg
  * Used free online image compression service from Kraken.io
  * File size of images/pizzeria.jpg and img/profilepic.jpg reduced to 5.97 KB and 4.62 KB, respectively
* In index.html, marked Google Analytics JS code to load asynchronously, so it is not render-blocking
* In index.html, inlined css/style.css
* In index.html, inlined the latin 400/700 fonts of Open Sans
  * For the web font, notice we only use the *latin* character set, for normal (font-weight: 400) and bold (font-weight: 700)
  * When downloading the original web font, we downloaded all possible character sets unnecessarily (e.g. extended Latin, Greek, Vietnamese, etc.)
* Minified index.html using http://www.willpeavy.com/minifier/
  * Saved original un-minified version at index_full.html, for reference

#### Part 2: Optimize Frames per Second in pizza.html

In part 2 of this project, I optimized views/js/main.js, such that views/pizza.html runs at 60 frames per second (fps) or higher, and the pizza container resizing operations complete in less than 5 milliseconds (ms).

##### Optimizations that I made

*All optimizations described below were implemented by modifying views/js/main.js*

Optimizations to allow 60 fps scrolling of views/pizza.html:
* Saved document.body.scrollTop to a local variable, and used this local variable in the subsequent for-loop
  * In the for loop that iterates through all moving background pizzas, we don't have to read from the document again, since we can now read from our local variable instead
  * Repeatedly reading from the document then writing to it causes layout thrashing, which we want to avoid

Optimizations to reduce time to resize pizza containers to less than 5 ms:
* Only calculate newwidth once
  * Originally, we were calculating newwidth everytime for each pizza container. However, for every pizza resize operation, the newwidth value is the same for each pizza container.
  * Thus, we only need to calculate newwidth once and save the value in a local variable, which saves us many document reads and calculations
* Saved the number of total pizza containers (i.e. document.querySelectorAll(".randomPizzaContainer").length) to a local variable, so the for-loop that uses this value does not have to read from the document for each iteration
* From the above modifications, the for-loop to iterate through all pizza containers now only includes document writes
  * No more document reads in this for-loop as before, which was causing layout thrashing
