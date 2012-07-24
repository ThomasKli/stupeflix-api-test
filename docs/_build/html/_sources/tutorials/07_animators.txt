.. _animators:

.. raw:: html

  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

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

Scale
-----

A scaling is performed using the "scale" key. The parameters are the scale along the X,Y and Z dimensions. The neutral value is of course 1.0.

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="custom">
      <key scale="1,1,1" time="0.0"/>
      <key scale="1.2,1,1" time="1.0"/>
      <key scale="1.0,1.2,1" time="4.0"/>
      <key scale="1,1,1" time="5.0"/>
    </animator>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="custom">
          <key scale="1,1,1" time="0.0"/>
          <key scale="1.2,1,1" time="1.0"/>
          <key scale="1.0,1.2,1" time="4.0"/>
          <key scale="1,1,1" time="5.0"/>
        </animator>
      </overlay>
  --></div>

Rotations
---------

A rotation is performed using the rot key. The first parameter in the rot value is the angle, and the following are the axis rotation vector. For example, 360,0,1,0 gives a 360 degrees rotation on the Y (up vertical) axis.

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="custom">
      <key rot="0,0,1,0" time="0.0"/>
      <key rot="360,0,1,0" time="5"/>
    </animator>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="custom">
          <key rot="0,0,1,0" time="0.0"/>
          <key rot="360,0,1,0" time="5"/>
        </animator>
      </overlay>
  --></div>

The four parameters are linearly interpolated from a key to the next one, so it may lead to some hard to understand behaviour if you specify things like :

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="custom">
      <key rot="0,0,1,0" time="0.0"/>
      <key rot="360,0,1,0" time="2.5"/>
      <key rot="360,0,0,1" time="5"/>
    </animator>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="custom">
          <key rot="0,0,1,0" time="0.0"/>
          <key rot="360,0,1,0" time="2.5"/>
          <key rot="360,0,0,1" time="5"/>
        </animator>
      </overlay>
  --></div>

As you can see, a 360 rotation is applied from 2.5s to 5s, so nothing moves.

For "continuous" rotations like this, you may use quaternion based rotations, using the orient parameter (see below).

Orientation
-----------

Orientation is different from rotation in the way it is interpolated. It is based on quaternions, so the rotation between two positions is more "natural", in the sense that it is the "shortest" rotation between the two positions. The parameters are the rotation on the X,Y and Z axes (Euler angles).

It can looks like a simple rotation in some cases:

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="custom">
       <key orient="0.0,0.0,0.0" time="0.0"/>
       <key orient="0.0,180.0,0,0" time="5.0"/>
    </animator>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="custom">
           <key orient="0.0,0.0,0.0" time="0.0"/>
           <key orient="0.0,180.0,0,0" time="5.0"/>
        </animator>
      </overlay>
  --></div>

Multiple values in keys
-----------------------

All keys in an animator must contains exactly the same set of attributes. For example, a key ``<key time="0.0" pos="0,0,0"/>`` is incompatible with a key ``<key time="0.0" pos="0,0,0" rot="360,0,0,0"/>``.

That said, you can combine several attributes in a key. They will be processed in this order: scale, pos, orient, rot.

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="custom">
       <key pos="0.0,0.0,0.0" time="0.0" rot="0.0,0.0,0.0,1.0"/>
       <key pos="0.0,0.0,-1.0" time="5.0" rot="360.0,0.0,0.0,1.0"/>
    </animator>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="custom">
           <key pos="0.0,0.0,0.0" time="0.0" rot="0.0,0.0,0.0,1.0"/>
           <key pos="0.0,0.0,-1.0" time="5.0" rot="360.0,0.0,0.0,1.0"/>
        </animator>
      </overlay>
  --></div>

Builtin animators
-----------------

Those animators are "shortcuts" to the most common operations.

Slide animators
---------------

The parameters are:

* direction : left, right, up, down (default is left)
* duration (default is full duration)

There are three kinds of slide animators : slide-in, slide-out, and slide.

``slide-in`` and ``slide-out`` are in/out animations. ``slide`` is animated during the full time.

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="slide-in"/>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="slide-in"/>
      </overlay>
  --></div>

Slide in with a duration of 1 second :

.. code-block:: xml

  <overlay duration="5.0">
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="slide-in" duration="1.0"/>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay duration="5.0">
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="slide-in" duration="1.0"/>
      </overlay>
  --></div>

Slide out with a duration of 1 second and direction is up :

.. code-block:: xml

  <overlay duration="5.0">
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="slide-out" direction="up" duration="1.0"/>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay duration="5.0">
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="slide-out" direction="up" duration="1.0"/>
      </overlay>
  --></div>

Slide:

.. code-block:: xml

  <overlay duration="5.0">
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="slide" direction="left"/>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay duration="5.0">
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="slide" direction="left"/>
      </overlay>
  --></div>

Of course you can combine several animations on the same overlay:

.. code-block:: xml

  <overlay duration="5.0">
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="slide-in" direction="left" duration="1.0"/>
    <animator type="slide-out" direction="down" duration="1.0"/>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay duration="5.0">
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="slide-in" direction="left" duration="1.0"/>
        <animator type="slide-out" direction="down" duration="1.0"/>
      </overlay>
  --></div>

Rotation animators
------------------

The parameters are:

* angle, in degrees
* duration (default is full duration)

There are two kind of rot animators : rot-in, rot-out.

These animators are similar to slide animators, but with rotations.

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="rot-in" angle="360" duration="1"/>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="rot-in" angle="360" duration="1"/>
      </overlay>
  --></div>

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="rot-out" angle="360" duration="1"/>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="rot-out" angle="360" duration="1"/>
      </overlay>
  --></div>

Grow animators
--------------

The parameters are:

* growStart, growEnd: default is 0.0, 0.1
* duration (default is full duration)

The effect is done by translating the object on the Z axis.

First, a grow effect on the full duration:

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="grow" growStart="-0.3" growEnd="0.0" />
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="grow" growStart="-0.3" growEnd="0.0" />
      </overlay>
  --></div>

Same thing, but shrinking:

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="grow" growStart="0.3" growEnd="0.0"/>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="grow" growStart="0.3" growEnd="0.0"/>
      </overlay>
  --></div>

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="grow" growStart="0.0" growEnd="0.1" duration="1"/>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="grow" growStart="0.0" growEnd="0.1" duration="1"/>
      </overlay>
  --></div>

Same thing at the end:

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="grow" growStart="0.1" growEnd="0.0" duration="1.0" margin-end="0.0"/>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="grow" growStart="0.1" growEnd="0.0" duration="1.0" margin-end="0.0"/>
      </overlay>
  --></div>

Animator combination
--------------------

An arbitrary number of animators can be chained. The previous example

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="custom">
       <key pos="0.0,0.0,0.0" time="0.0" rot="0.0,0.0,0.0,1.0"/>
       <key pos="0.0,0.0,-1.0" time="5.0" rot="360.0,0.0,0.0,1.0"/>
    </animator>
  </overlay>

is equivalent to this one:

.. code-block:: xml

  <overlay>
    <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
    <animator type="custom">
       <key pos="0.0,0.0,0.0" time="0.0"/>
       <key pos="0.0,0.0,-1.0" time="5.0"/>
    </animator>
      <animator type="custom">
       <key rot="0.0,0.0,0.0,1.0" time="0.0"/>
       <key rot="360.0,0.0,0.0,1.0" time="5.0"/>
    </animator>
  </overlay>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <overlay>
        <image filename="http://assets.stupeflix.com/code/images/surf.jpg"/>
        <animator type="custom">
           <key pos="0.0,0.0,0.0" time="0.0"/>
           <key pos="0.0,0.0,-1.0" time="5.0"/>
        </animator>
          <animator type="custom">
           <key rot="0.0,0.0,0.0,1.0" time="0.0"/>
           <key rot="360.0,0.0,0.0,1.0" time="5.0"/>
        </animator>
      </overlay>
  --></div>

Node hierarchy and animators
----------------------------

You can use animators at several places in your XML tree, each one will animate a node in the "scene graph".

.. code-block:: xml

  <stack>
    <animator type="custom">
       <key pos="0.0,0.0,-3.0" time="0.0"/>
    </animator>
    <animator type="custom">
       <key rot="0.0,0.0,1.0,0.0" time="0.0"/>
       <key rot="30.0,1.0,1.0,0.0" time="1.666"/>
       <key rot="-30.0,1.0,1.0,0.0" time="3.3333"/>
       <key rot="0.0,0.0,1.0,0.0" time="5.0"/>
    </animator>

    <overlay>
      <image filename="http://assets.stupeflix.com/code/images/Monument_Valley.jpg"/>
      <animator type="custom">
         <key pos="-2.8,2.1,0.0" time="0.0"/>
         <key pos="-1.8,1.1,0.0" time="1.0"/>
      </animator>
    </overlay>
    <overlay>
      <image filename="http://assets.stupeflix.com/code/images/Ha_long_bay.jpg"/>
      <animator type="custom">
         <key pos="2.8,2.1,0.0" time="0.0"/>
         <key pos="1.8,1.1,0.0" time="1.0"/>
      </animator>
    </overlay>

     <overlay left="auto" right="auto" height="1.0">
      <image filename="http://assets.stupeflix.com/code/images/Lightning_over_Oradea_Romania.jpg"/>
      <animator type="custom">
         <key pos="-2.8,-2.1,0.0" time="0.0"/>
         <key pos="-1.8,-1.1,0.0" time="1.0"/>
      </animator>
    </overlay>


    <overlay>
      <image filename="http://assets.stupeflix.com/code/images/Canyon_de_Chelly_Navajo.jpg"/>
      <animator type="custom">
         <key pos="2.8,-2.1,0.0" time="0.0"/>
         <key pos="1.8,-1.1,0.0" time="1.0"/>
      </animator>
    </overlay>

  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack>
        <animator type="custom">
           <key pos="0.0,0.0,-3.0" time="0.0"/>
        </animator>
        <animator type="custom">
           <key rot="0.0,0.0,1.0,0.0" time="0.0"/>
           <key rot="30.0,1.0,1.0,0.0" time="1.666"/>
           <key rot="-30.0,1.0,1.0,0.0" time="3.3333"/>
           <key rot="0.0,0.0,1.0,0.0" time="5.0"/>
        </animator>

        <overlay>
          <image filename="http://assets.stupeflix.com/code/images/Monument_Valley.jpg"/>
          <animator type="custom">
             <key pos="-2.8,2.1,0.0" time="0.0"/>
             <key pos="-1.8,1.1,0.0" time="1.0"/>
          </animator>
        </overlay>
        <overlay>
          <image filename="http://assets.stupeflix.com/code/images/Ha_long_bay.jpg"/>
          <animator type="custom">
             <key pos="2.8,2.1,0.0" time="0.0"/>
             <key pos="1.8,1.1,0.0" time="1.0"/>
          </animator>
        </overlay>

         <overlay left="auto" right="auto" height="1.0">
          <image filename="http://assets.stupeflix.com/code/images/Lightning_over_Oradea_Romania.jpg"/>
          <animator type="custom">
             <key pos="-2.8,-2.1,0.0" time="0.0"/>
             <key pos="-1.8,-1.1,0.0" time="1.0"/>
          </animator>
        </overlay>


        <overlay>
          <image filename="http://assets.stupeflix.com/code/images/Canyon_de_Chelly_Navajo.jpg"/>
          <animator type="custom">
             <key pos="2.8,-2.1,0.0" time="0.0"/>
             <key pos="1.8,-1.1,0.0" time="1.0"/>
          </animator>
        </overlay>

      </stack>
  --></div>

Finally, you can mix custom and builtin animators, they have exactly the same role:

.. code-block:: xml

  <stack>
    <animator type="custom">
       <key pos="0.0,0.0,-3.0" time="0.0"/>
    </animator>
    <animator type="custom">
       <key rot="0.0,0.0,1.0,0.0" time="0.0"/>
       <key rot="30.0,1.0,1.0,0.0" time="1.666"/>
       <key rot="-30.0,1.0,1.0,0.0" time="3.3333"/>
       <key rot="0.0,0.0,1.0,0.0" time="5.0"/>
    </animator>

    <overlay>
      <image filename="http://assets.stupeflix.com/code/images/Monument_Valley.jpg"/>
       <animator type="custom">
         <key pos="-1.8,1.1,0.0" time="0.0"/>
      </animator>
      <animator type="slide-in" direction="left" duration="1.0"/>
    </overlay>
    <overlay>
      <image filename="http://assets.stupeflix.com/code/images/Ha_long_bay.jpg"/>
      <animator type="custom">
         <key pos="1.8,1.1,0.0" time="0.0"/>
      </animator>
      <animator type="slide-in" direction="down" duration="1.0"/>
    </overlay>

     <overlay left="auto" right="auto" height="1.0">
      <image filename="http://assets.stupeflix.com/code/images/Lightning_over_Oradea_Romania.jpg"/>
      <animator type="custom">
        <key pos="-1.8,-1.1,0.0" time="0.0"/>
      </animator>
      <animator type="slide-in" direction="right" duration="1.0"/>
     </overlay>


    <overlay>
      <image filename="http://assets.stupeflix.com/code/images/Canyon_de_Chelly_Navajo.jpg"/>
       <animator type="custom">
        <key pos="1.8,-1.1,0.0" time="0.0"/>
      </animator>
      <animator type="rot-in" angle="90" duration="1.0"/>
    </overlay>

  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack>
        <animator type="custom">
           <key pos="0.0,0.0,-3.0" time="0.0"/>
        </animator>
        <animator type="custom">
           <key rot="0.0,0.0,1.0,0.0" time="0.0"/>
           <key rot="30.0,1.0,1.0,0.0" time="1.666"/>
           <key rot="-30.0,1.0,1.0,0.0" time="3.3333"/>
           <key rot="0.0,0.0,1.0,0.0" time="5.0"/>
        </animator>

        <overlay>
          <image filename="http://assets.stupeflix.com/code/images/Monument_Valley.jpg"/>
           <animator type="custom">
             <key pos="-1.8,1.1,0.0" time="0.0"/>
          </animator>
          <animator type="slide-in" direction="left" duration="1.0"/>
        </overlay>
        <overlay>
          <image filename="http://assets.stupeflix.com/code/images/Ha_long_bay.jpg"/>
          <animator type="custom">
             <key pos="1.8,1.1,0.0" time="0.0"/>
          </animator>
          <animator type="slide-in" direction="down" duration="1.0"/>
        </overlay>

         <overlay left="auto" right="auto" height="1.0">
          <image filename="http://assets.stupeflix.com/code/images/Lightning_over_Oradea_Romania.jpg"/>
          <animator type="custom">
            <key pos="-1.8,-1.1,0.0" time="0.0"/>
          </animator>
          <animator type="slide-in" direction="right" duration="1.0"/>
         </overlay>


        <overlay>
          <image filename="http://assets.stupeflix.com/code/images/Canyon_de_Chelly_Navajo.jpg"/>
           <animator type="custom">
            <key pos="1.8,-1.1,0.0" time="0.0"/>
          </animator>
          <animator type="rot-in" angle="90" duration="1.0"/>
        </overlay>

      </stack>

  --></div>

.. raw:: html

  <script type="text/javascript" charset="utf-8" src="http://static.stupeflix.com/play/1.2/play-min.js"></script>