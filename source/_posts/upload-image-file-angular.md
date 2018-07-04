---
title: Upload an Image as a File in Angular
tags:
  - angular
  - file upload
  - images
url: 4373.html
id: 4373
categories:
  - Angular 2
date: 2017-07-04 06:30:08
---

This past week, I needed to be able to upload an image in my application to the server as a file so that I could crop it and upload it. Now, uploading an image that you pulled up using the file upload control is relatively straight forward.  But, in our case, the image we want to be able to upload didn’t always come from the user’s file system.  This causes two problems. First, you can’t crop an image you retrieved from a different URL using the HTML Canvas because of Cross Origin restrictions and second, you can’t upload the file using the standard file upload mechanism because you didn’t get it from the file system. <figure>![](/uploads/2017/07/2017-07-04.jpg "Upload an Image as a File in Angular") Photo via [Visual Hunt](//visualhunt.com/re/3bc74d)</figure>

<!-- more -->

Image to Data without Canvas
----------------------------

Now, the standard way of converting an Image to a data URL is to

*   create a new Image,
*   set the onload event handler to a function
*   set the Image src attribute to the file
*   in the onload function,
    *   draw the image onto the canvas
    *   call canvas.toDataUrl(mimeType) to get the data url.

It is pretty trivial code: https://gist.github.com/DaveMBush/0454717c390db1055674ff8a2817f68d#file-canvas-example-ts But, the trouble starts when you use a URL that doesn’t originate from the file system, or the same domain as the application your are running. The trick is to read the image using an XMLHttpRequest for foriegn URLs and then use a FileReader object and call readAsDataURL passing the result of the XMLHttpRequest.read. https://gist.github.com/DaveMBush/0454717c390db1055674ff8a2817f68d#file-data-url-ts Now we can put the image using the data URL on the canvas and the canvas doesn’t know we got it from a foreign URL any more so we no longer get a cross origin URL. The entry point for this code is on line 27.  You see  that if we are working with a File, we just use the FileReader directly.  But if we are using http, we go through the XMLHttpRequest.

Fake File Upload
----------------

The only problem with this is that now we no longer have the file point, so if we want to upload the file, as a file, to the server we have come up with some way of creating a fake file object.  And believe it or not, that is a lot easier than you might think. You see, a File object is just a kind of Blob object.  So, all we really need to do is to create a Blob.  But, we have one additional issue.  Our image is in base 64 and we need to convert it to a binary byte array. https://gist.github.com/DaveMBush/0454717c390db1055674ff8a2817f68d#file-b64tofile-ts You can use the resulting “File” anywhere you would use a File you had retrieved from the file system.

Upload via Http
---------------

The last bit of this is that we need to upload this via the Http service.  This is going to be harder to show with code because it depends on what you need to do. In my case, I needed to just upload the file, so the post was pretty straight forward.  I used an Http.post() and passed the returned “file” as the data parameter. But, you may need to upload it by wrapping the file in a Form object and specifying varying headers.

The End
-------

I know it is a relatively short post, but I hope it is helpful to someone.  There are a lot of examples out there of how to do this in JavaScript and jQuery, but I was unable to find anything that was specific to TypeScript and Angular.
