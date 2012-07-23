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

XML Editor is an online tool that allow you to write your video XML and test it immediatly: `xeditor.stupeflix.com <xeditor.stupeflix.com>`_.


Play.js
=======

Stupeflix Play enables you to include Stupeflix XML right into your HTML and automatically transform it into a video.

Get the current version
-----------------------
2 files must be included in your web pages in order to use the Stupeflix Play service.

Css Theme
+++++++++
The Stupeflix Play css theme is hosted at:
``http://static.stupeflix.com/play/1.2/style.css``
Copy the following code, then paste onto the ``<head>`` tag of your web page::
  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

Javascript Client
+++++++++++++++++
The Stupeflix Play Javascript Client is hosted at: ``http://static.stupeflix.com/play/1.2/play.js``
Copy the following code, then paste it immediately before the ``</body>`` tag of your web page::
  <script type="text/javascript">
    document.write(unescape('%3Cscript src="http://static.stupeflix.com/play/1.2/play-min.js" type="text/javascript"%3E%3C/script%3E'));
  </script>

A quick example
---------------
Use ``div`` elements with the ``sxmovie`` css class to include Stupeflix XML in your web pages.
Don't forget to give those divs a size, it will be used as your movie size.
Stupeflix XML must be included in a comment node: ``<!-- -->``::
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

he language is based on **XML**, and use the `Craftsman 1.0 API schema <http://code.stupeflix.com/schema.xsd>`_.