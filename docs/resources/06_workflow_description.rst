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

.. note::
  createProfiles can launch the generation of multiple profiles for the same definition, but they are after that totally independent, so you can start downloading/using them independently too.

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
