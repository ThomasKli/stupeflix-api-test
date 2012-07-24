*********
Tutorials
*********

Stupeflix API
=============

Automatically send videos
-------------------------

To Youtube, Facebook, Dailymotion, Amazon S3, my own server ...

Using the Stupeflix API, you can choose the kind of video you want to generate, through a XML file which is send by POST by the createProfiles function. But in the same call, you can choose too to upload the video to several targets : Amazon S3 bucket, http server, and even YouTube, Facebook or Dailymotion. Here are some examples given for PhP. Other languages are supported too, as clients are only a wrappers to generate a XML settings file. The XML Schema can be found here: http://code.stupeflix.com/createprofile.xsd . To have these examples running, you will need the Stupeflix SDK.

Generic code
++++++++++++

Here is a skeleton for video creation / upload. The video definition you want to send should be in the "test.xml" file:

.. code-block:: php

  <?php
    define('stupeflixAccessKey', 'PUT-YOUR-ACCESS-KEY-HERE');
    define('stupeflixSecretKey', 'PUT-YOUR-SECRET-KEY-HERE');
  
    // Create the client
    $stupeflix = new Stupeflix(stupeflixAccessKey, stupeflixSecretKey);
  
    // If you plan to offer the service to your own users,
    // you can set the user variable to your user id.
    // Otherwise, you can always keep the same name
    $user = "user112";
  
    // Each of your users can have different projects.
    // Each resource id should be unique to each project, as video file will be overwritten at each video generation
    $resource = "resource2034";
  
    $metaDict = array("title" => "Upload test ", 
                      "description"=>"This is an upload test", 
                      "thumbnailTime"=>"2",  // useful to choose the thumbnail time
                      "tags"=>"upload test",
                      "channels"=>"Tech",
                      "acl"=>"public",
                      "location"=>"48.8583,2.2945"); // A famous tower position
                    
    $meta = new StupeflixMeta($metaDict);
  
    $upload = #################### HERE IS THE UPLOAD SETUP CODE
  
    $profile = new StupeflixProfile("iphone", array($upload), $meta); 
  
    $profiles = new StupeflixProfileSet(array($profile), null, null);
    $stupeflix->sendDefinition($user, $resource, "test.xml");
    $stupeflix->createProfiles($user, $resource, $profiles);
  ?>

For the following upload code bits, you should replace the $upload = ... line by the provided code. This example show you how to upload to a single target, but you can define several upload objects at once this way:

.. code-block:: php

  <?php
    $upload0 = #################### HERE IS THE UPLOAD SETUP CODE
    $upload1 = #################### HERE IS THE UPLOAD SETUP CODE
    $upload2 = #################### HERE IS THE UPLOAD SETUP CODE
  
    $profile = new StupeflixProfile("iphone", array($upload0, $upload1, $upload2), $meta); 
  ?>

Stupeflix Default Upload
++++++++++++++++++++++++

The code is simply:

.. code-block:: php

  <?php
    $upload = new StupeflixDefaultUpload();
  ?>

The video will be stored temporarily on Stupeflix storage. You have to use this upload type if you want to be able to retrieve the video through a getProfile API call. **This upload is required if you want to use the getProfileUrl/getProfile functions in the stupeflix clients. If you don't add it, the url returned with getProfileUrl will give you a 404 as the file does not exist at this place.**

Youtube Upload
++++++++++++++

The first way to use the youtube upload is simply to use the login / password of an account:

.. code-block:: php

  <?php
    $upload = new StupeflixYoutubeUpload('MY_YOUTUBE_LOGIN', 'MY_YOUTUBE_PASSWORD', $meta);
  ?>

This is of course not always possible: - if you don't want us to know your Youtube credentials - if you want to use your own users credentials and they don't want YOU to have them The Youtube API allows you to create / get a one time or session token. ( http://code.google.com/apis/youtube/2.0/developers_guide_protocol.html#AuthSub_Authentication ) So we provide a second way: you to pass the token to us, so we can upload the video without knowing the full credentials. In that case, the code is:

.. code-block:: php

  <?php
    $upload = new StupeflixYoutubeTokenUpload('MY_YOUTUBE_DEVELOPER_KEY', 'MY_TOKEN');
  ?>

Facebook Upload
+++++++++++++++

Uploading videos to Facebook is similar to Youtube upload. A different point is that meta information is limited to title and description. And something important to be noted is that every uploaded video is by default visible to everybody, not limited to friends. There is currently no mean to change it through their API, so we can't either offer it to our users. So basically, the code is:

.. code-block:: php

  <?php
    $upload = new StupeflixFacebookUpload("MY_FACEBOOK_API_KEY", "MY_APPLICATION_SECRET", "MY_USER_TOKEN", $meta);
  ?>

Your Facebook API Key can be safely shared, so it is OK to give to us. The application secret is different, as you should not give it to unsafe parties (by the way, I am not meaning that we are unsafe ;-) ) This secret is different if your APP is registered as a Web App or a Desktop App on Facebook. (By the way those names are not really well chosen, as you Web App can use as well a "Desktop App" style of authentification). So, if your application is a Web App, you have to send us your application secret. But if your application is a Desktop App, you HAVE to create a session secret, and send it to us. Don't send your application secret, as it won't work. Desktop App authentication is designed to run in untrusted environement, that's exactly what we needed. Finally, you send us a user session token you have built using the Facebook API. **Some python example for facebook authentication code can be found** `Here <http://wiki.stupeflix.com/doku.php?id=facebookuploadauthpython>`_.

Amazon S3 Upload
++++++++++++++++

A simple way to send videos to your own bucket is to use the following code ( DISCLAIMER: THIS FIRST METHOD IS STRICLY NOT RECOMMENDED BY AMAZON, AS YOUR SECRET KEY SHOULD BE KEPT SECRET AT ALL TIME. ):

.. code-block:: php

  <?php
    $upload = new StupeflixS3Upload("MY_BUCKET_NAME", "MY_ACCESS_KEY", "MY_SECRET_KEY", "MY_PREFIX", $meta);
  ?>

Where you fill in your Amazon S3 accesskey and secret key. The video will be stored under the "MY_PREFIX_PATH" A SAFE way it to grant write access to your bucket to the Stupeflix Application, using bucket ACLS. Documentation for ACLS can be found [[http://docs.amazonwebservices.com/AmazonS3/latest/dev/index.html?S3_ACLs.html|here]]. To do so, got to https://console.aws.amazon.com/s3/home and set the bucket ACL with write access to Stupeflix editing the properties of the bucket, using our canonical id : ef5245b3f16862462f8e735597a41f668003600ceaea3b4e86a94e5fee1d5f82 . The code is then:

.. code-block:: php

  <?php
    $upload = new StupeflixS3Upload("MY_BUCKET_NAME", null, null, "MY_PREFIX", $meta);
  ?>

A thumbnail will be uploaded alongwith the video.

Dailymotion Upload
++++++++++++++++++

To perform an upload on Dailymotion, simply use the login / password of an account:

.. code-block:: php

  <?php
    $upload = new StupeflixDailymotionUpload('MY_DAILYMOTION_LOGIN', 'MY_DAILYMOTION_PASSWORD', $meta);
  ?>

HTTP PUT Upload
+++++++++++++++

The code is simply:

.. code-block:: php

  <?php
    $upload = new StupeflixHttpPUTUpload("http://path/to/my/upload/script");
  ?>

A PUT call will be done on this url, with the video in the body.

HTTP POST Upload
++++++++++++++++

The code is simply:

.. code-block:: php

  <?php
    $upload = new StupeflixHttpPOSTUpload("http://path/to/my/upload/script");
  ?>

A POST call will be done on this url, with the video in a multipart field whose name is "file".

Send a zip file to the api instead of a simple XML file
-------------------------------------------------------

Here is a `sample zip file <https://stupeflix-assets.s3.amazonaws.com/code/samples/ziptest.zip>`_. Each asset kind must be stored in its own sub-directory: **images, audios, videos, fonts** At the root of the zip file you must add a **movie.xml** file conforming to the Stupeflix Video XML Language and describing the video. The assets are named in the movie.xml file without specifying their directory prefix:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack>
        <audio filename="music.mp3" duration=".."/>
        <sequence>
          <effect type="diving" >
            <image filename="image1.jpg"/>
            <image filename="image2.png"/>
          </effect>
          <text type="zone" fontname="myfont">Coucou</text>
          <effect type="none" duration="5">
            <video filename="video.mp4"/>
          </effect>
         </sequence>
       </stack>
    </body>
  </movie>

For example the video.mp4 is actually stored in the video directory. The structure of the zip file is simple:

.. code-block:: guess

  movie.xml
  audios/
    music.mp3
  fonts/
    myfont
  images
    image1.jpg
    image2.png
  videos
    video.mp4


Stupeflix XML Language
======================

Introduction
------------

The Stupeflix Video Description Language is an XML language (called SXML below).

It is intended to be as simple as possible to create videos, yet powerful enough to allow complex projects. It was designed around several **fundamental ideas**:

* it is XML, and fully human readable
* it is 4D
* it is vector-based

XML, and fully human readable
+++++++++++++++++++++++++++++

A lot of software claims that they are able to export XML documents. Sometimes it is true and sometimes the XML contains actually chunk of binary data, in hexadecimal form, if you are lucky. That's not the most useful form of XML most developers would say.

So even if in the end most of our customers use scripts to generate SXML bits, the XML is always human readable. So, you can understand and debug it easily. You can share snippets of XML to show how to create a nice effect you designed. You can use all the standard tools to create XML or to process it. XSLT stylesheets are for example a good way to build video templates that will transform your data into a SXML file.

The description hierarchy is a good fit with the XML node hierarchy, and I hope you will find it natural to use. I do, but you know, I am not really that objective on this subject.

And, finally, you can even decide to rewrite a video rendering engine using the same XML language of you want. Don't forget to contact us if you do so!

More seriously, your projects will have some chance to be used in another place, in another time, outside the Stupeflix platform.

4D
++

Everything is 3D in SXML, even 2D elements. And even more, everything is 4D, as everything can be animated.

So, if you decide to animate your scene, your 2D elements will in fact reveal as 3D planes, just as every other ones. In a similar way, if you decide to alter filters parameters throughout a sequence, just add an animator as child element in the XML. Each node in the hierarchy can be animated: think of the SXML as a large scene graph, so if you animate one node, all sub-elements will move in 3D.

Vector based video description
++++++++++++++++++++++++++++++

Vector based graphics are well known. That means that you can scale up them without a loss of precision. In the SXML language, the fundamental idea is the same. However, the idea is pushed a little bit further.

The same SXML file can be used to generate different videos, at different resolution. That's nice, that's exactly the kind of things Vector based graphics are good at. BUT, an issue quickly emerges: what about aspect ratio ? How do you render the video with a 16:9 ratio for a plasma screen, a 4:3 ratio for a web video, or 3:2 ratio for an iPhone ? Do you stitch the video ? It would be ugly.

The answer is that everything in the language should be liquid to adapt to the video aspect ratio, much more a web page than classical vector graphics. When you position your overlays, you specify margins, just like in CSS. When you add images in an effect, their aspect ratio is kept. When you use animators, you can ignore the screen ratio and just say "slide-in" or "slide-out".

But maybe the most important concept is about time: you won't find any timeline in the SXML. Instead, you can nest video sequence in the language itself, and container duration will be computed from its content. For example, you want to create videos using a fixed template and with some voice over. The voice over duration is different each time. In a standard timeline based video editor, you would have to move around video elements to adapt to the new audio duration. With the SXML, just includes the voice over, its duration will be detected before rendering starts, and propagated throughout the full video description. You can even download the XML with every node annotated with duration information, just by using a special API call, for debugging purpose for example.

You will see throughout the upcoming tutorials other examples of this important concept.

Conclusion
++++++++++

So, that's all for today. Next time, we will talk less about philosophy, and more about action !

Masking
=======

Masking & Keying
----------------

Masking and Keying are very important compositing operators. They are used everywhere, from the weather forecast clip to blockbusters. The basic idea is to let a given color become transparent when you compose an image on top another one. There are two ways to proceed in the SXML language.

Applying a mask transform an existing image and set some color to partly or totally transparent. The mask itself is a bicolor image, one being the opaque one, and the other one is transparent. Any intermediary color will result in a partly transparent color.

So, when you apply a mask on an image, it just modifies the alpha channel of it. It is particularly useful as video do not usally have an alpha channel.

Finally, you can apply it in two ways: on a single image, or on the full screen.

Image filtering
---------------

Here are the media we are using. First the video to be displayed: `sts120_launch.mov <http://assets.stupeflix.com/code/tutorials/masking/sts120_launch.mov>`_.

Then the mask `wbmask.mp4 <http://assets.stupeflix.com/code/tutorials/masking/wbmask.mp4>`_.

We combine it using a filter with type "mask". White will be opaque, whereas black will be fully transparent:

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay duration="10.0" top="0.1">
        <video filename="http://assets.stupeflix.com/code/tutorials/masking/sts120_launch.mov">
          <filter type="mask" opaqueColor="#ffffff" transparentColor="#000000">
            <video filename="http://assets.stupeflix.com/code/tutorials/masking/wbmask.mp4"/>
          </filter>
        </video>
      </overlay>
  --></div>

In fact we used SXML too to build the mask :

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack duration="10">
        <overlay right="0.15" width="0.45" top="0.15" height="0.45">
           <image color="#ffffff"/>
           <animator type="slide-in" duration="1.0" direction="left"/>
           <animator type="slide-out" duration="1.0" direction="up"/>
           <animator type="grow" growEnd="0.7"/>
        </overlay>
        <overlay left="0.10" width="0.45" top="0.10" height="0.45">
          <image color="#ffffff"/>
          <animator type="slide-in" duration="1.0" direction="down"/>
          <animator type="slide-out" duration="1.0" direction="left"/>
          <animator type="grow" growEnd="0.7"/>
        </overlay>
        <overlay left="0.15" width="0.45" bottom="0.10" height="0.45">
          <image color="#ffffff"/>
          <animator type="slide-in" duration="1.0" direction="up"/>
          <animator type="slide-out" duration="1.0" direction="left"/>
          <animator type="grow" growEnd="0.7"/>
        </overlay>
        <overlay right="0.10" width="0.45" bottom="0.15" height="0.45">
          <image color="#ffffff"/>
          <animator type="slide-in" duration="1.0" direction="left"/>
          <animator type="slide-out" duration="1.0" direction="down"/>
          <animator type="grow" growEnd="0.7"/>
        </overlay>
      </stack>
  --></div>

And finally, if we want to, we can combine the xmls into a single one, as we can nest filters and rendering nodes (stack, sequence, overlay, effect etc).

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay duration="10.0" top="0.1">
        <video filename="http://assets.stupeflix.com/code/tutorials/masking/sts120_launch.mov">
          <filter type="mask" opaqueColor="#ffffff" transparentColor="#000000">
            <stack duration="10.0">
              <overlay right="0.15" width="0.45" top="0.15" height="0.45">
                <image color="#ffffff"/>
                <animator type="slide-in" duration="1.0" direction="left"/>
                <animator type="slide-out" duration="1.0" direction="up"/>
                <animator type="grow" growEnd="0.7"/>
              </overlay>
              <overlay left="0.10" width="0.45" top="0.10" height="0.45">
                <image color="#ffffff"/>
                <animator type="slide-in" duration="1.0" direction="down"/>
                <animator type="slide-out" duration="1.0" direction="left"/>
                <animator type="grow" growEnd="0.7"/>
              </overlay>
              <overlay left="0.15" width="0.45" bottom="0.10" height="0.45">
                <image color="#ffffff"/>
                <animator type="slide-in" duration="1.0" direction="up"/>
                <animator type="slide-out" duration="1.0" direction="left"/>
                <animator type="grow" growEnd="0.7"/>
              </overlay>
              <overlay right="0.10" width="0.45" bottom="0.15" height="0.45">
                <image color="#ffffff"/>
                <animator type="slide-in" duration="1.0" direction="left"/>
                <animator type="slide-out" duration="1.0" direction="down"/>
                <animator type="grow" growEnd="0.7"/>
              </overlay>
            </stack>
          </filter>
        </video>
      </overlay>
  --></div>

Scene Post-Processing
---------------------

As we just saw, you can apply mask to a single image, in an overlay, text or effect.

But you can apply it too as a postprocessing. In that case it will be full screen, and you apply it every rendering node (text, overlay, effect, stack, sequence).

You can see the result at `fullscreen_mask_video.mp4 <http://assets.stupeflix.com/code/tutorials/masking/fullscreen_mask_video.mp4>`_.

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack duration="10.0">
        <filter type="mask" opaqueColor="#ffffff" transparentColor="#000000">
          <video filename="http://assets.stupeflix.com/code/tutorials/masking/wbmask.mp4"/>
        </filter>
        <effect type="sliding">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Lightning_over_Oradea_Romania_3.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
        </effect>
      </stack>
  --></div>

As previously, you can insert inline a rendering node instead of the wbmask video, for example a stack containing several overlays.


Audio, Voice Over & Text-To-Speech
==================================

Audio
-----

What would be video without audio ? Fortunately, handling multiple audio tracks is built-in the SXML.

So what can you do ?

* integrate an arbitrary number of audio tracks in the stack-sequence hierarchy
* superpose several audio tracks at the same time
* adjust volume
* add fade-in / fade-out effects
* skip the first n seconds in the audio track

In addition, you can also produce audio tracks, using speech synthesis. Finally, by default, every video you add in an effect or an overlay come with its own audio track, which is mixed by default with the other audio tracks.

Basic use, and a common mistake
+++++++++++++++++++++++++++++++

You may want to add an audio track to your video using something similar to:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <effect type="diving">
        <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
        <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
        <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Lightning_over_Oradea_Romania_3.jpg"/>
      </effect>
      <audio filename="http://assets.stupeflix.com/code/tutorials/audio/soundtracks/Flypaper.mp3"/>
    </body>
  </movie>

But this is not the way it works : just see the result at `audio_sequence.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_sequence.mp4>`_.

The "body" xml node is a sequence, and so the audio will be played after the effect finishes, not at the same time. The effect will use its default duration, 5 seconds.

So the correct way to do so is to add a stack to your xml and place your effect and your audio in it.

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack>
        <effect type="diving">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
        </effect>        
        <audio filename="http://assets.stupeflix.com/code/tutorials/audio/soundtracks/Flypaper.mp3"/>
      </stack>
    </body>
  </movie>

Just remember this point, and using audio will be really easy.

Here is the result: `audio_fullduration.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_fullduration.mp4>`_.

As you can see, the total duration of the video is 3:20, the duration of the music.

Is this really what we wanted ? Not really. So, let see how to change this.

Multiple tracks: audio as slave
+++++++++++++++++++++++++++++++

So how can you add several audio at the same time, and how do you force them to adopt the length of the rest of video elements ? Just use the xml structure to do so:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <!-- All music by Josh Woodward, The Simple Life -->
      <stack>
        <effect type="diving" duration="10">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
        </effect>        
        <audio filename="http://assets.stupeflix.com/code/tutorials/audio/soundtracks/Flypaper.mp3" duration=".." fadein="2"/>
      </stack>
      <transition type="swirl" duration="2.0"/>
      <stack>
        <effect type="square" duration="15">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
        </effect>
        <audio filename="http://assets.stupeflix.com/code/tutorials/audio/soundtracks/ImLettingGo.mp3" duration=".." fadeout="2"/>
      </stack>
    </body>
  </movie>

You can find the result here : `audio_slave.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_slave.mp4>`_.

This way, the audio is bound to play with the effect.

Note the ``duration=".."`` directive in the audio tags: it bounds the duration of the audio track to its parent xml node duration. The stack will look for its children to determine its duration, and so it will ignore the audio tag, as this latter rely itself on its parent. So, that's the effect node that fix the duration. If you remove this directive, the effect and the stack node duration will be fixed by the audio duration.

You can see some sample use of fadein and fadeout directives.

Note that the transition has an effect on audio too : it creates automatically a crossfade between the two soundtracks.

Multiple tracks: audio as master
++++++++++++++++++++++++++++++++

So, adding a soundtrack is easy. But what about adding a voiceover when you do not know how long it is because it's created using text to speech ? You can play with durations the other way round:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <!-- All music by Josh Woodward, The Simple Life -->
      <stack>
        <effect type="diving">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
        </effect>        
        <audio filename="http://assets.stupeflix.com/code/tutorials/audio/soundtracks/Flypaper.mp3" duration=".." fadein="2" fadeout="2" volume="0.1" skip="5"/>
        <audio voice="neospeech:julie" margin-end="2.0">Music by Josh Woodward, Flypaper, from the album The Simple Life. Available on Jamendo dot com. Images from Wikipedia Picture of the Day.</audio>
      </stack>
    </body>
  </movie>

Here is the result: `audio_master.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_master.mp4>`_.

In that sample, the music duration is set to ".." , as previously. But we removed the duration from the effect. So, what is the duration of the stack, effect, and music ? It's the duration of the last audio track, the voice over. Duration is obtained from the audio file produced by the speech synthetizer. Note the margin-end added to the voice over, to allow some extra time at the end of the voice over and before the end of the video. You can add the same way a "margin-start" directive if you want. Finally, we used the volume directive on the music, to put the emphasis on the voice rather than on the music, and we skipped the first 5 seconds of the music.

Audio sequences
+++++++++++++++

You can easily concatenate different audio tracks. Just create as sequence of audio files. Here we use speech synthesis, but it is exactly the same with standard audio files.

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack>
        <effect type="diving" duration="10">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
        </effect>        
        <sequence>
           <audio voice="neospeech:julie">Hello.</audio>
           <audio voice="neospeech:paul">Who are you ?</audio>
           <audio voice="neospeech:julie">I am Julie. Don't you remember ?</audio>
           <audio voice="neospeech:paul">No, definitely not.</audio>
        </sequence>
      </stack>
    </body>
  </movie>

Here is the result: `audio_sequence.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_sequence.mp4>`_.

Using video with audio
++++++++++++++++++++++

Remember, when you add a video to an effect or to an overlay, an audio track is automatically created. You can prevent the audio track to be created by specifying audio="false" in the video node.

Let's see some examples:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <effect type="diving" duration="10">
        <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
        <video filename="http://assets.stupeflix.com/code/tutorials/audio/video/sts120_launch.mov"/>
      </effect>
    </body>
  </movie>

The result is here : `audio_video.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_video.mp4>`_.

How does it works ? The diving effect has a specified duration, so it imposes it to its children. The video will play after roughly 5 seconds, for 5 seconds, and so will do the related audio track.

Now, let's remove the audio:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <effect type="diving" duration="10">
        <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
        <video filename="http://assets.stupeflix.com/code/tutorials/audio/video/sts120_launch.mov" audio="false"/>
      </effect>
    </body>
  </movie>

See it in action : `audio_video_noaudio.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_video_noaudio.mp4>`_.

Finally, let's add an audio track from a video:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack>
        <audio filename="http://assets.stupeflix.com/code/tutorials/audio/video/sts120_launch.mov" fadein="2" fadeout="2" duration=".."/>
        <effect type="diving" duration="10">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <video filename="http://assets.stupeflix.com/code/tutorials/audio/video/sts120_launch.mov" audio="false"/>
        </effect>        
      </stack>
    </body>
  </movie>

Remember, we have to add a stack to be able to superpose the audio with the effect. Then, we have to add ``duration=".."`` to the audio, so it is a slave of its "stack" parent. And finally, we set ``audio="false"`` on the video.

See it now: `audio_video_asaudio.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_video_asaudio.mp4>`_.

That was actually a good recapitulation of what we saw earlier today.

You said easy? I heard you!

Overlay
=======

Overlays are just like text nodes. They can be placed on screen using CSS like parameters :

* top, bottom, left, right
* width, height

The only difference with text nodes is that you can add to them an image or a video: the aspect ratio of the image/video will be kept, so the overlay may not be completely filled by its content.

You can then animate the overlay using the same animators as for text.

Sample XML movie:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack duration="10.0">
        <effect type="none">
          <image color="#eeeeee"/>
        </effect>
        <overlay left="0.1" width="0.3" top="0.1" height="0.3" margin-start="1.0" duration="4.0">
          <image color="#ff00ff"/>
          <animator type="slide-in" direction="up" duration="1.0" />          
        </overlay>
        <overlay right="0.1" width="0.3" top="0.3" height="0.3" margin-start="3.0" duration="4.0">
          <image color="#0000ff"/>
          <animator type="slide-in" direction="down" duration="1.0" />
          <animator type="slide-out" direction="right" duration="1.0" />
        </overlay>

        <overlay right="0.35" width="0.3" top="0.35" height="0.3" margin-start="6.0" duration="4.0">
          <image color="#888888"/>
          <animator type="slide-in" direction="down" duration="1.0" />
          <animator type="slide-out" direction="right" duration="1.0" />
          <animator type="grow" growStart="0.0" growEnd="1.0"/>
        </overlay>

      </stack>
    </body>
  </movie>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
        <movie service="craftsman-1.0">
          <body>
            <stack duration="10.0">
              <effect type="none">
                <image color="#eeeeee"/>
              </effect>
              <overlay left="0.1" width="0.3" top="0.1" height="0.3" margin-start="1.0" duration="4.0">
                <image color="#ff00ff"/>
                <animator type="slide-in" direction="up" duration="1.0" />          
              </overlay>
              <overlay right="0.1" width="0.3" top="0.3" height="0.3" margin-start="3.0" duration="4.0">
                <image color="#0000ff"/>
                <animator type="slide-in" direction="down" duration="1.0" />
                <animator type="slide-out" direction="right" duration="1.0" />
              </overlay>

              <overlay right="0.35" width="0.3" top="0.35" height="0.3" margin-start="6.0" duration="4.0">
                <image color="#888888"/>
                <animator type="slide-in" direction="down" duration="1.0" />
                <animator type="slide-out" direction="right" duration="1.0" />
                <animator type="grow" growStart="0.0" growEnd="1.0"/>
              </overlay>

            </stack>
          </body>
        </movie>
  --></div>

Text effects
============

Warning: this is still a bit experimental. It will be supported in next releases, so you can use it right now for production deployment. Rendering quality will be improved for specific cases, but it is already a large improvement compared to the previous bitmap based method, as text rendering is now vector-based on a frame-by-frame basis. So, you may say, why it is experimental ? Because we will package it in a better way, so you don't have to manage the nitty-gritty details we describe here. Be brave, that won't be so difficult.

Reminder: text rendering
------------------------

Historically, rendering text is done this way:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <text type="zone" fontcolor="#ffffff">
        Hello World
      </text>
    </body>
  </movie>

This is a full screen "Hello World". The principle is simple: the text is turned into an image once for all, and then used as standard image. The drawback is evident: if you zoom on the text it is blurry and aliased.

Text rendering: new method
--------------------------

So how can we improve this ? By drawing frame by frame the text but using vector based methods. To activate it, you should change your xml to:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <text type="zone" vector="true">
        Hello World
        <filter type="distancemap" distanceWidth="40.0"/>
        <filter type="distancecolor" distanceWidth="40.0" color="#ffffff" />
      </text>
    </body>
  </movie>

This is a bit verbose, but will be improved in next versions. You can use 40.0 as magic constant, it should be ok in most cases.

The difference should be almost invisible right now. But if you animate the text (zoom, scale ...), the difference will be obvious.

Effects
-------

Quality improvement is an interesting property. But the most important benefit comes from the availability of a lot of new effects. All the effects are configured in the distancecolor filter: you just have to replace it with your own one. Simple white text:

.. code-block:: xml

  <filter type="distancecolor" distanceWidth="40.0" color="#ffffff" />

Drop Shadow
+++++++++++

Arguments are:

* dropShadowColor : RGBA (red, green, blue, alpha in hexadecimal)
* dropShadowBlurWidth: 0.0 for hard shadow, 1.0 for fully blurred
* dropShadowOpacity : multiplification on alpha channel of dropShadowColor
* dropShadowPosition : x,y position, in text zone height units (1.0 is so equal to the text zone height)
* dropShadowAngle / dropShadowDistance : alternative to dropShadowPosition, but in polar coordinates

.. code-block:: xml

  <filter type="distancecolor" distanceWidth="40.0" color="#0000ff" dropShadowColor="#000000" dropShadowBlurWidth="0.5" dropShadowOpacity="0.8" dropShadowPosition="-0.02,0.02"/>


Sample video:

.. raw:: html

  <video controls="" src="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/texteffects/dropshadow.mp4"></video>

Inner Shadow
++++++++++++

Arguments are:

* innerShadowColor : RGBA (red, green, blue, alpha in hexadecimal)
* innerShadowBlurWidth: 0.0 for hard shadow, 1.0 for fully blurred
* innerShadowOpacity : multiplification on alpha channel of dropShadowColor
* innerShadowPosition : x,y position, in text zone height units (1.0 is so equal to the text zone height)
* innerShadowAngle / innerShadowDistance : alternative to innerShadowPosition, but in polar coordinates

.. code-block:: xml

  <filter type="distancecolor" distanceWidth="40.0" color="#cccccc" innerShadowColor="#000000" innerShadowBlurWidth="0.5" innerShadowOpacity="0.8" innerShadowPosition="-0.005,0.005"/>

Sample video:

.. raw:: html

  <video controls="" src="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/texteffects/innershadow.mp4"></video>

Stroke Color
++++++++++++

Arguments are:

* strokeWidth : 0.0 for no stroke, 1.0 for maximum width
* strokeOpacity : 0.0 for totally transparent, 1.0 for fully opaque
* strokeColor : color for stroke

.. code-block:: xml

  <filter type="distancecolor" distanceWidth="40.0" color="#cccccc" strokeWidth="0.04" strokeOpacity="1.0" strokeColor="#0000FF" /> 

Sample video:

.. raw:: html

  <video controls="" src="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/texteffects/stroke.mp4"></video>

Outer Glow Color
++++++++++++++++

Arguments are:

* outerGlowBlurWidth : 0.0 for no glow, 1.0 for maximum glow width
* outerGlowOpacity : 0.0 for totally transparent, 1.0 for fully opaque
* outerGlowColor : color for glow

.. code-block:: xml

  <filter type="distancecolor" distanceWidth="40.0" color="#cccccc" outerGlowBlurWidth="1.0" outerGlowOpacity="0.5" outerGlowColor="#FF0000" />

Sample video:

.. raw:: html

  <video controls="" src="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/texteffects/outerglow.mp4"></video>

Inner Glow Color
++++++++++++++++

Arguments are:

* innerGlowBlurWidth : 0.0 for no glow, 1.0 for maximum glow width
* innerGlowOpacity : 0.0 for totally transparent, 1.0 for fully opaque
* innerGlowColor : color for glow

.. code-block:: xml

  <filter type="distancecolor" distanceWidth="40.0" color="#cccccc" innerGlowBlurWidth="0.03" innerGlowOpacity="0.5"  innerGlowColor="#FF0000" />

Sample video:

.. raw:: html

  <video controls="" src="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/texteffects/innerglow.mp4"></video>

Full Example
------------

Now, you can combine those effects to create a composite one.

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack duration="20">
        <overlay height="1.0" width="1.0">
          <image color="#ffffff" /> 
        </overlay>
        <text type="zone" vector="true">
          Hello World
          <filter type="distancemap" distanceWidth="40.0"/>
          <filter type="distancecolor" distanceWidth="40.0" color="#eda35f" 
                  strokeColor="#000000" strokeOpacity="1.0" strokeWidth="0.02"
                  innerShadowColor="#de7316" innerShadowOpacity="1.0" innerShadowPosition="0.01,-0.01"
                  dropShadowColor="#00000044" dropShadowOpacity="1.0" dropShadowBlurWidth="0.9" dropShadowPosition="0.05,-0.05"
                  outerGlowColor="#ffffff44" outerGlowOpacity="1.0" outerGlowBlurWidth="0.7">
          </filter>
        </text>
      </stack>
    </body>
  </movie>

Sample video:

.. raw:: html

  <video controls="" src="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/texteffects/composite.mp4"></video>

Another sample, with a scale animation to show good antialiasing properties:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack duration="20">
        <overlay height="1.0" width="1.0">
          <image color="#ffffff" /> 
        </overlay>
        <text type="zone" vector="true">
          Hello World
          <filter type="distancemap" distanceWidth="40.0"/>
          <filter type="distancecolor" distanceWidth="40.0" color="#eda35f" 
                  strokeColor="#000000" strokeOpacity="1.0" strokeWidth="0.02"
                  innerShadowColor="#de7316" innerShadowOpacity="1.0" innerShadowPosition="0.01,-0.01"
                  dropShadowColor="#00000044" dropShadowOpacity="1.0" dropShadowBlurWidth="0.9" dropShadowPosition="0.05,-0.05"
                  outerGlowColor="#ffffff44" outerGlowOpacity="1.0" outerGlowBlurWidth="0.7">
          </filter>
          <animator type="custom" >
            <key time="0.0" scale="0.25,0.25,0.25" pos="0,0,0"/>
            <key time="20.0" scale="3,3,3" pos="0,0.5,0.0"/>
          </animator>
        </text>
      </stack>
    </body>
  </movie>

Sample video:

.. raw:: html

  <video controls="" src="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/texteffects/composite_scale.mp4"></video>

Extra Bonus: animation
----------------------

The nice thing with all these properties is that everything can be animated. You just have to move the properties to animate into an animator.

Here, we take the last example, but we are starting from no stroke to slight stroke, and we are moving the drop shadow:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack duration="10">
        <overlay height="1.0" width="1.0">
          <image color="#ffffff" /> 
        </overlay>
        <text type="zone" vector="true">
          Hello World
          <filter type="distancemap" distanceWidth="40.0"/>
          <filter type="distancecolor" distanceWidth="40.0" color="#eda35f" 
                  strokeColor="#000000" strokeOpacity="1.0"
                  innerShadowColor="#de7316" innerShadowOpacity="1.0" innerShadowPosition="0.01,-0.01"
                  dropShadowColor="#00000044" dropShadowOpacity="1.0" dropShadowBlurWidth="0.9"
                  outerGlowColor="#ffffff44" outerGlowOpacity="1.0" outerGlowBlurWidth="0.7">
            <animator type="custom">
              <key time="0.0" strokeWidth="0.0" dropShadowPosition="0.05,-0.05" />
              <key time="10.0" strokeWidth="0.02" dropShadowPosition="-0.05,-0.05" />
            </animator>
          </filter>
        </text>
      </stack>
    </body>
  </movie>

Sample video:

.. raw:: html

  <video controls="" src="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/texteffects/composite_parameter_animate.mp4"></video>

The good news is that rendering is just as fast as when nothing is animated. No less, no more. Thanks for your attention, and feel free to send us feedback about your experience on this feature!

Animators
=========

Animators can be used in two different contexts:

* geometric transformations animations: translations, rotations, scale
* parameters animations in filters

Geometric animations
--------------------

Geometric animators can be inserted in any node that is drawn or any container

* sequence (or body, which is only the top level sequence)
* stack
* effect
* text
* overlay
* widget
* sphere
* camera

A arbitrary number of animators can be stacked in the same node. The geometric transformations will be then applied in the given order, except for the camera where the order is reversed.

The OpenGL coordinate system is used:

.. image:: http://assets.stupeflix.com/code/images/cartesian.png

Coordinates for the full screen are:

* x in [-aspectRatio, + aspectRatio]
* y in [-1, +1]
* z = 0

Where aspectRatio is for example 16/9 = 1.7777777... in the following examples.

There are two kinds of animators: builtins, and custom. The first ones are already packaged for you. The latter ones are user defined.

Custom animators
----------------

They can be used by specifying the "custom" animator type.

.. code-block:: xml

  <animator type="custom">
     <!-- ... -->
  </animator>

You can then add some **key** xml node in it. Each key must contain a time attribute, expressed in seconds, and an arbitrary combination of attributes "rot", "orient", "pos", or "scale" (one of each maximum).

Translations
------------

A translation is performed using the "pos" key. The parameters are the translation along the X,Y and Z dimensions.

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="custom">
      <key pos="-3.55555555,0,0" time="0.0"/>
      <key pos="0,0,0" time="1.0"/>
      <key pos="0,0,0" time="4.0"/>
      <key pos="3.55555555,0,0" time="5.0"/>
    </animator>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="custom">
          <key pos="-3.55555555,0,0" time="0.0"/>
          <key pos="0,0,0" time="1.0"/>
          <key pos="0,0,0" time="4.0"/>
          <key pos="3.55555555,0,0" time="5.0"/>
        </animator>
      </overlay>
      -->
  </div>
