*********
Resources
*********

Developer Kit
=============

Python Client
-------------
By using `pip <http://www.pip-installer.org/>`_, install the Stupeflix Python Client:
``pip install stupeflix_api``

PHP, Ruby, and Java Clients
---------------------------
Zip download: `Download <https://github.com/Stupeflix/Stupeflix-API-Client/zipball/master>`_

Git download:
``git clone https://github.com/Stupeflix/Stupeflix-API-Client.git``

Checkout the lastest version of these client sources on `Stupeflix's Github <https://github.com/Stupeflix/Stupeflix-API-Client>`_.
Then follow the instructions on the README file.

Other language
--------------
Simply use our `REST API <http://developer.stupeflix.com/documentation/rest_api>`_.


XML Editor
==========

XML Editor is an online tool that allow you to write your video XML and test it immediatly: `xeditor.stupeflix.com <http://xeditor.stupeflix.com>`_.


Play.js
=======

Stupeflix Play enables you to include Stupeflix XML right into your HTML and automatically transform it into a video.

Get the current version
-----------------------
2 files must be included in your web pages in order to use the Stupeflix Play service.

Css Theme
+++++++++

The Stupeflix Play css theme is hosted at: http://static.stupeflix.com/play/1.2/style.css

Copy the following code, then paste onto the ``<head>`` tag of your web page:

.. code-block:: html

  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

Javascript Client
+++++++++++++++++

The Stupeflix Play Javascript Client is hosted at: http://static.stupeflix.com/play/1.2/play.js

Copy the following code, then paste it immediately before the ``</body>`` tag of your web page:

.. code-block:: html

  <script type="text/javascript">
    document.write(unescape('%3Cscript src="http://static.stupeflix.com/play/1.2/play-min.js" type="text/javascript"%3E%3C/script%3E'));
  </script>

A quick example
---------------

Use ``div`` elements with the ``sxmovie`` css class to include Stupeflix XML in your web pages.

Don't forget to give those divs a size, it will be used as your movie size.

Stupeflix XML must be included in a comment node: ``<!-- -->``:

.. code-block:: xml

  <div class="sxmovie" style="width:640px; height:360px;"><!--
    <effect type="explode" duration="20">
      <video filename="http://stupeflix-website.s3.amazonaws.com/code/nasa.mov"/>
    </effect>
    <transition type="move" direction="left" duration="2"/>
    <effect type="flower" duration="20">
      <video skip="18" filename="http://stupeflix-website.s3.amazonaws.com/code/nasa.mov"/>
    </effect>
    <transition type="move" direction="left" duration="2"/>
    <effect type="slice" duration="20">
      <video skip="36" filename="http://stupeflix-website.s3.amazonaws.com/code/nasa.mov" />
    </effect>
  --></div>

Video Description Language
==========================

The language is based on **XML**, and use the `Craftsman 1.0 API schema <http://code.stupeflix.com/schema.xsd>`_.

Example
-------

Let's start with an example of a very simple but yet valid movie definition.

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack>
        <effect type="kenburns" duration="2.5">
          <image filename = "http://assets.stupeflix.com/code/images/Ha_long_bay.jpg"/>
        </effect>
        <effect type="panel" duration="5.0">
          <image filename = "http://assets.stupeflix.com/code/images/Canyon_de_Chelly_Navajo.jpg"/>
        </effect>
        <audio filename="http://dl.dropbox.com/u/1524046/Music/Catch_a_Marvel.mp3" fadeout="2" />
        <text type="legend">Welcome to the desert.</text>
      </stack>
      <effect type="flower" duration="5.0">
        <image filename = "http://assets.stupeflix.com/code/images/Monument_Valley.jpg"/> 
      </effect>
    </body>
  </movie>

Alternatively, you can choose to pack all the assets and movie definition in a single file, as for the stupeflix-1.0 format. 
This xml description produce a movie made of two parts: - first part with a stack of several elements : fullscreen foreground with a kenburns effect disappearing after 2.5 seconds, and fullscreen background with a panel effect, plus a text effect on top of it. An audio track is added, with a fadeout of "2s". - second part, a single flower effect, with no audio Let's see now how to build a video using this service.

Movie definition, full featured
-------------------------------

To describe a movie, we will start by defining atoms, that creates a video stream or an audio stream, and operators to compose these elements. Operators transform one or several video/audio streams into one stream which can be further combined with other streams.

Atoms
+++++

Atoms are responsible for generating a single video / audio stream directly from assets. They define a stream of audio or video (or both) with a fixed duration. They will be combined on the fly by the video generator.

Available atoms are:
``effect``
Can be used to turn one or several images or video assets (image and video tags) into a video stream (with optional audio stream).
``text``
Turn a text into a video stream, using some kind of text rendering effect.
``audio``
Turn an audio asset into an audio stream.
``overlay``
Add an overlay on top of an existing scene.

NB
++

Image and video tags are not directly atoms, as the effect is responsible to handle aspect ratio adaptation if the input image or video aspect ratio is different from the generated video.

Operators
+++++++++

An operator combines several audio/video streams into a single one.

Available operators are:

``sequence``
This operator takes a set of streams and just concatenate them in time. The related tag is sequence, or body : top level tag is a sequence.
``stack``
This operator takes a set of streams and display them together. The display order is the xml order : first in the list, first to be drawn, and so may be partially hidden by subsequent draws.

Atoms / Operators combination
+++++++++++++++++++++++++++++

A stack or a sequence or effect can contain atoms, sequences, and stacks.

Effects
+++++++

You will find a complete descriptions of parameters for each kind of effects in http://wiki.stupeflix.com/doku.php?id=effects.

A typical effect tag looks like:

.. code-block:: xml

  <effect type="diving"  duration="5.0" >
    <image filename="http://assets.stupeflix.com/code/images/Ha_long_bay.jpg"/>
    <image filename="http://assets.stupeflix.com/code/images/Canyon_de_Chelly_Navajo.jpg"/>
    <image filename="http://assets.stupeflix.com/code/images/Monument_Valley.jpg"/>
  </effect>

Transitions
+++++++++++

A transition tag can only be inserted in a sequence tag (or in the body tag). It uses the previous and the next video sequences provided, overlap them (reducing the sequence total duration), and so generate a new video stream. Previous and next video sequences can be any video stream: effect or stack of complex effects.

You will find a complete descriptions of parameters for each kind of transition in http://wiki.stupeflix.com/doku.php?id=transitions.

A typical transition tag looks like :

.. code-block:: xml

  <transition type="move" direction="down" duration="1" />

Text
++++

============= ============ ========================================================================================================
Attribute     Presence     Description
============= ============ ========================================================================================================
type          required     the type of text effect. The only currently available type is "legend".
duration      optional     the duration of the text tag (optional because it may be inferred from the enclosing stack tag).
============= ============ ========================================================================================================

The text to be displayed is then contained in the text tag:

.. code-block:: xml

  <text type="legend">Here is the text to be displayed</text>

Here is `the list of available text effects <http://wiki.stupeflix.com/doku.php?id=texteffects>`_ and the `list of available fonts <http://wiki.stupeflix.com/doku.php?id=fonts>`_.

============= ============ ========================================================================================================
Attribute     Presence     Description
============= ============ ========================================================================================================
filename      required     the filename in the zip //soundtracks// directory or a url
duration      optional     overrides the audio file original duration
fadein        optional     starts the audio track with a fade in of the specified duration
fadeout       optional     ends the audio track with a fade out of the specified duration
skip          optional     skip the specified duration in the input file
margin-start  optional     starts the audio track with the specified delay (full duration is usually given by a enclosing node)
margin-end    optional     ends the audio track with the speficied delay before the end (full duration given by the enclosing node)
margin-both   optional     sets both margin. This is exclusive of other "margin" attributes
volume        optional     arbitrary float, sets the volume (default is 1.0)
============= ============ ========================================================================================================

An audio entry example:

.. code-block:: xml

  <audio filename="thriller.mp3" fadein="1.0" fadeout="4.0" skip="2.0" />

Here is a small example on how to use margins:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack>
        <effect type="panel" duration="15.0">
          <image filename = "http://assets.stupeflix.com/code/images/Ha_long_bay.jpg"/>
        </effect>
        <audio filename="http://dl.dropbox.com/u/1524046/Music/Catch_a_Marvel.mp3" fadeout="2.0" margin-both="2.0" />
      </stack>
    </body>
  </movie>

In that case, the stack tag is assigned the only specified duration, 15.0 seconds. So the audio margins apply on that duration, and audio track is composed of:
* 2.0 seconds of silence
* 9.0 seconds of music
* 2.0 seconds of fade out
* 2.0 seconds of silence

For a total of 15.0 seconds.

More information on how durations are affected in `Computing Durations <http://wiki.stupeflix.com/doku.php?id=computingdurations>`_.

Images and Videos
-----------------

Images
++++++

============= ============ ========================================================================================================
Attribute     Presence     Description
============= ============ ========================================================================================================
filename      required     the filename in the zip //images// directory or a url.
color         required     the "#" char followd by a 6 or 8 long RGB(A) hexadecimal string
============= ============ ========================================================================================================

So an image examples are:

.. code-block:: xml

  <image filename="lena.jpg"/>
  <image color="#ff0000"/>
  <image color="#ffffff88"/>

You can too query Google Maps to get directly a `map image <http://wiki.stupeflix.com/doku.php?id=gmapsimage>`_.

Videos
++++++

============= ============ ========================================================================================================
Attribute     Presence     Description
============= ============ ========================================================================================================
filename      required     the filename in the zip //videos// directory or a url.
============= ============ ========================================================================================================

Example:

.. code-block:: xml

  <video filename="sunset.mpg"/>

Filters
-------

More
++++

You will find a complete descriptions of parameters for each kind of filter in `Filters <http://wiki.stupeflix.com/doku.php?id=filters>`_.

Supported coders, formats
=========================

Protocols
---------

Supported protocols
+++++++++++++++++++

for filename xml attributes

* HTTP
* HTTPS

Available video profiles
------------------------

=================== ===================== =========== ================= ================= ======================= ==============
profile name        width x height        framerate   video bit rate    audio bit rate    video/audio codecs      file extension
=================== ===================== =========== ================= ================= ======================= ==============
iphone              480×320               25 fps      700k              96k               h264/aac                mp4
youtube             640×360               25 fps      1024k             128k              h264/aac                mp4
quicktime           640×480               25 fps      1024k             128k              h264/aac                mp4
quicktime-small     320×240               25 fps      256k              64k               h264/aac                mp4
flash-small         320×240               25 fps      192k              64k               flv/mp3                 flv
iphone-flv          480×320               25 fps      1024k             96k               flv/mp3                 flv
youtube-flv         640×360               25 fps      1700k             128k              flv/mp3                 flv
flash               640×480               25 fps      512k              96k               flv/mp3                 flv
flash-hq            640×480               25 fps      1200k             128k              flv/mp3                 flv
mobile-small        176×144               15 fps      92k               32k               mpeg4/aac               mp4
mobile              320×240               25 fps      256k              32k               mpeg4/aac               mp4
wmv1                640×480               25 fps      1024k             128k              wmv1/wmav1              wmv
wmv2                640×480               25 fps      1024k             128k              wmv2/wmav1              wmv
dvd-ntsc-4-3        720×480               29.97 fps   dvd               dvd               mpeg2/mpega             mpg
dvd-ntsc-4-3-h      352×240               29.97 fps   dvd               dvd               mpeg2/mpega             mpg
dvd-ntsc-16-9       720×480               29.97 fps   dvd               dvd               mpeg2/mpega             mpg
dvd-ntsc-16-9-h     352×240               29.97 fps   dvd               dvd               mpeg2/mpega             mpg
dvd-pal-4-3         720×576               25 fps      dvd               dvd               mpeg2/mpega             mpg
dvd-pal-4-3-h       352×288               25 fps      dvd               dvd               mpeg2/mpega             mpg
dvd-pal-16-9        720×576               25 fps      dvd               dvd               mpeg2/mpega             mpg
dvd-pal-16-9-h      352×288               25 fps      dvd               dvd               mpeg2/mpega             mpg
720p                1280×720              25 fps      1500k             256k              h264/aac                mp4
=================== ===================== =========== ================= ================= ======================= ==============

Supported assets
----------------

Images
++++++

* jpeg
* png
* gif

Videos
++++++

* mp4, flv, wmv, quicktime …
* h264, flash, mpeg2 …

Soundtrack
++++++++++

* mp3
* wav

Workflow Description
====================

Overview
--------

The sequence of **operations** to create a video is:

* sendDefinition : create a new definition file
* createProfiles : launch generation of a profile, that means a video based on the definition but with given size, codec, bitrate and specific settings for `notification <http://wiki.stupeflix.com/doku.php?id=notification>`_, uploads etc.

At this point you can use two methods to **get information on rendering**:

* getStatus : **get status** for a single / a set of profiles from the API (you may query multiple status at the same time)
* wait for a `notification <http://wiki.stupeflix.com/doku.php?id=notification>`_ from the API on your own web server if you specified a callback url in createProfiles settings

When the status say a profile is available, you can then retrieve it using:

* getProfile : **download a profile**
* getProfileThumb : **download a thumbnail**

You can too use getProfileURL to retrieve only the url of a profile. You just have to remember that the url **won't be valid for more than 24 hours**, and that the **video may be destroyed** after that, as we do not offer long term hosting for videos created with the API. (Other Stupeflix solutions may includes hosting, see our solutions).

Alternatively, you can tell the API that the video should be **directly uploaded to your site**, through HTTP POST.

Other upload types (HTTP PUT, specific YouTube account, Amazon S3) will be added in the future: this is specified by the settings you specified in createProfiles. If this is the case, you are done!

NB: createProfiles can launch the generation of multiple profiles for the same definition, but they are after that totally independent, so you can start downloading/using them independently too.

Resource IDs
------------

The API= identify each client using a pair AccessKey / SecretKey.

Each video definition is uniquely identified by a **user/resource** path.

Then, when you select a profile for rendering, each obtained video is uniquely identified by the **user/resource/profile** path.

Each client space is totally private : accessing the same user / resource or profile using different access keys will yield different results.

Each client space is totally private : accessing the same user / resource or profile using different access keys will yield different results.

User and resources are intended to simplify the developer work.

You can choose different strategies, for example:

* You can map your own users on the User variable (that was designed for)
* If you don't have users, or you don't need that feature, you can use a single user and create different resources.

Identifiers
-----------

=============== ========= =================================
Variable        Length    Regexp
=============== ========= =================================
accessKey       20        [a-zA-Z0-9]{20}
secretKey       40        [a-zA-Z0-9]{40}
user            1-48      [a-zA-Z0-9]{1,48}
resource        1-256     [a-zA-Z0-9]{1,256}
profile         1-16      see Supported coders, formats
=============== ========= =================================

REST API
========

REST
----

That's fine, it means it's easy to create client libraries for a lot of different languages. But it's clearly not enough to know how to use the API ...

URL Schema
++++++++++

Parameters:

* Date
* AccessKey
* Signature

Available resources
+++++++++++++++++++

The REST philosophy says that everything should be a resource. So here is the list of resources:

**Definition**
  A `zip file containing assets and a xml description file <http://wiki.stupeflix.com/doku.php?id=zippedmoviefiles>`_, or directly a xml file with assets referred by url.

**Profile**
A video generated by the service, using a specific codec, resolution and container. Available profiles are listed in `Supported Formats <http://wiki.stupeflix.com/doku.php?id=supportedformats>`_.

**Status**
A json object giving information on a set of objects (currently only profiles).

Basic WorkFlow
++++++++++++++

1. Create a new definition using a PUT on a definition resource url

2. Create a video profile using A POST with an profile creation directive XML file on a definition resource URL. The XML file must comply with the schema.

3. Get the status from the profile status resource url

4. Check its content

5. When the main status is "available", you can download the video. If the profile creation directive file specified some upload targets on Youtube, the video was already uploaded there, so this step is optional.

Signing requests
++++++++++++++++

Request signature is built using the same kind of schema as Amazon web services. Have a look at the provided client libraries for more information. (documentation coming soon)