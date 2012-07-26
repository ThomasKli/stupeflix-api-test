.. _play_js:

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
