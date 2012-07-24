.. _overlay:

.. raw:: html

  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

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

.. raw:: html

  <script type="text/javascript" charset="utf-8" src="http://static.stupeflix.com/play/1.2/play-min.js"></script>