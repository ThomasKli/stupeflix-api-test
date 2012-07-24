*******
Support
*******

===
FAQ
===

General
-------

What kind of applications can I develop?
++++++++++++++++++++++++++++++++++++++++

  The API can be used to develop services on the web, or on mobile platforms like the iPhone. You can think of applications such as widgets for social platforms, video creation tool for photo hosting services, automated creation of news videos using a flow of pictures...

Can I only create slideshows?
+++++++++++++++++++++++++++++

  The API can be used to easily create videos from still images, with some simply selectable effects and transitions, and an optional overlay for adding your logo for example. This was the Stupeflix API v1.0 xml language.

  This was the first goal of the API. Still, the API has involved in a much more powerful tool, available as a new xml language. With this language, you can arbitrarily combine effects, transition, audio tracks and videos.

How can I use it?
+++++++++++++++++

  The API is available as a REST web service. It is inspired by one of the most famous web servies, Amazon S3: the way the web service is accessed is very similar. This allows every device with a web access to be able to use the API. You just have to use the API to upload a video definition file, which describes the content of the video. You will need an API key to be able to use: just register as a developer on the web site to get your key. Client libraries are currently available for php, python, and Rails is coming soon. Feel free to add languages to this list: this is a REST service, it is very easy to adapt to your language!

What is a video definition file?
++++++++++++++++++++++++++++++++

  The definition file may be just an xml description file, with the assets referred within it using urls, or a zip file containing both the xml description file and some or all assets contained in the zip file (you can always use urls to refer to assets). If your assets are all available online and does not require authentication for access, we recommend that you use the xml only format : you will just have to upload a few kilobytes of text, and not a large zip with the assets. Use the zip method if some of your assets are not available online.

Video Generation
----------------

How long is the video generation?
+++++++++++++++++++++++++++++++++

  Video generation time is dependent mostly on the output resolution and codec. For a DVD resolution, a one minute video will be generated in one minute. For a small format (320x240), generation time will be 30 seconds, depending too on the size of the assets you are using as an input.

Where are the videos generated?
+++++++++++++++++++++++++++++++

  The video are generated on our servers. So, you have nothing to install on your side.

What kind of formats can I create?
++++++++++++++++++++++++++++++++++

  You can use mpeg-2, mpeg-4, h264, 3gp codecs , with flv, mp4, avi, mov, dvd formats (this is not limitative). See `Supported coders, formats <http://developer.stupeflix.com/documentation/supported_coders_formats>`_ for an exhaustive list.

What is the maximum duration of a video?
++++++++++++++++++++++++++++++++++++++++

  There is **no limit** on the number of images, soundtracks you use as inputs. There is **no limit** on the final video duration.

Video Hosting
-------------

Are you hosting videos?
+++++++++++++++++++++++

  We are just storing the videos for a limited amount of time so you can download it. A lot of very good video hosting services are available, we "just" provide a tool to create videos!

For how long are my video definition files hosted?
++++++++++++++++++++++++++++++++++++++++++++++++++

  The video definition files are kept on our server one month. So if you want to re-generate the same video with a better profile (higher resolution, better codec), you don't have to reupload the definition, just launch a new video profile generation.

For how long are my video hosted?
+++++++++++++++++++++++++++++++++

  Videos are kept for 1 day after they are generated.

=======
Contact
=======

`Contact us <http://studio.stupeflix.com/contact/>`_