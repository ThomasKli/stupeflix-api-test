.. _stupeflix_api:

Stupeflix API
=============

Automatically send videos
-------------------------

To Youtube, Facebook, Dailymotion, Amazon S3, my own server...

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

For the following upload code bits, you should replace the ``$upload = ...`` line by the provided code. This example show you how to upload to a single target, but you can define several upload objects at once this way:

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