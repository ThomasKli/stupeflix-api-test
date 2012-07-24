.. _text_effects:

.. raw:: html

  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

Text effects
============

Warning: this is still a bit experimental. It will be supported in next releases, so you can use it right now for production deployment. Rendering quality will be improved for specific cases, but it is already a large improvement compared to the previous bitmap based method, as text rendering is now vector-based on a frame-by-frame basis. So, you may say, why it is experimental? Because we will package it in a better way, so you don't have to manage the nitty-gritty details we describe here. Be brave, that won't be so difficult.

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

So how can we improve this? By drawing frame by frame the text but using vector based methods. To activate it, you should change your xml to:

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

The difference should be almost invisible right now. But if you animate the text (zoom, scale...), the difference will be obvious.

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

.. raw:: html

  <script type="text/javascript" charset="utf-8" src="http://static.stupeflix.com/play/1.2/play-min.js"></script>