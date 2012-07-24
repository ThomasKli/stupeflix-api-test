.. _time_margins:

.. raw:: html

  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

Time Margins
============

Explanation
-----------

Duration computation is an important part of the Stupeflix Video XML Language processing.

Margins are there to position in time the different parts of a video. The three parameters are called ``margin-start`` ``margin-end`` and ``margin-both``.

Margins are relative to a container. This container can be anything except a sequence (or body, which is a sequence too).

Margins are the time equivalent to CSS margins. They must be greater than or equal to zero.
Four values are related by the following: ``fatherDuration = margin-start + duration + margin-end``

So, if you specify only margin-start or margin-end, duration will take all the remaining time. If you specify margin-start and duration, margin-end will be computed accordingly. If fatherDuration is fixed, then some part of the children may be truncated.

You cannot specify at the same time margin-start, duration and margin-end, or margin-both and duration. You will have to specify instead fatherDuration and two of the three parameters.

Let's start with some examples.

Basic examples
--------------

The total video length will be 4.0 seconds, the first second is black, the last two seconds are black too.

.. code-block:: xml

  <stack duration="4.0">
    <overlay  margin-start="1.0" margin-end="2.0" width="1.0">
      <image filename="http://assets.stupeflix.com/code/images/Monument_Valley.jpg"/>
    </overlay>
  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack duration="4.0">
        <overlay  margin-start="1.0" margin-end="2.0" width="1.0">
          <image filename="http://assets.stupeflix.com/code/images/Monument_Valley.jpg"/>
        </overlay>
      </stack>
  --></div>

The total video length will be 4.0 seconds, the first two seconds are black.

.. code-block:: xml

  <stack duration="4.0">
    <overlay  duration="2.0" margin-end="0.0" width="1.0">
      <image filename="http://assets.stupeflix.com/code/images/Monument_Valley.jpg"/>
    </overlay>
  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack duration="4.0">
        <overlay  duration="2.0" margin-end="0.0" width="1.0">
          <image filename="http://assets.stupeflix.com/code/images/Monument_Valley.jpg"/>
        </overlay>
      </stack>
  --></div>

Element duration dependencies
-----------------------------

In this example, the duration is fixed by the video element (taking into account that the video is speedup by a factor of 2.0), and so the text duration will be adapted, using video duration decreased by the margins both at start and at the end.

.. code-block::  xml

  <stack>
    <overlay width="1.0">
      <video filename="http://assets.stupeflix.com/code/tutorials/masking/sky_timelapse.mp4" speed="2.0"/>
    </overlay>
    <text type="zone" height="0.1" margin-both="2.0">Sky Time Lapse</text> 
  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack>
        <overlay width="1.0">
          <video filename="http://assets.stupeflix.com/code/tutorials/masking/sky_timelapse.mp4" speed="2.0"/>
        </overlay>
        <text type="zone" height="0.1" margin-both="2.0">Sky Time Lapse</text> 
      </stack>
  --></div>

In a second example, we use margins on animators for the text. The actual duration of the text is

.. code-block:: xml

  <stack>
    <overlay width="1.0">
      <video filename="http://assets.stupeflix.com/code/tutorials/masking/sky_timelapse.mp4" speed="2.0"/>
    </overlay>
    <text type="zone" height="0.1" left="0.02">Sky Time Lapse
      <animator type="slide-in" direction="left" margin-start="1.5" duration="2.0"/>
      <animator type="slide-out" direction="left" margin-end="1.5" duration="1.0"/>
    </text> 
  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack>
        <overlay width="1.0">
          <video filename="http://assets.stupeflix.com/code/tutorials/masking/sky_timelapse.mp4" speed="2.0"/>
        </overlay>
        <text type="zone" height="0.1" left="0.02">Sky Time Lapse
          <animator type="slide-in" direction="left" margin-start="1.5" duration="2.0"/>
          <animator type="slide-out" direction="left" margin-end="1.5" duration="1.0"/>
        </text> 
      </stack>
  --></div>

In the next example, we use margins on the filter : the filter tint is stuck on the white color only after one second. The animator is in fact 1 second shifted.

.. code-block:: xml

  <stack>
    <overlay width="1.0">
      <video filename="http://assets.stupeflix.com/code/tutorials/masking/sky_timelapse.mp4" speed="2.0"/>
      <filter type="tint">
         <animator type="custom" margin-start="1.0">
           <key time="0.0" whiteColor="#ffffff"/>
           <key time="3.0" whiteColor="#00ff00"/>
           <key time="6.0" whiteColor="#ffff00"/>
         </animator>
      </filter>   
    </overlay> 
  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack>
        <overlay width="1.0">
          <video filename="http://assets.stupeflix.com/code/tutorials/masking/sky_timelapse.mp4" speed="2.0"/>
          <filter type="tint">
             <animator type="custom" margin-start="1.0">
               <key time="0.0" whiteColor="#ffffff"/>
               <key time="3.0" whiteColor="#00ff00"/>
               <key time="6.0" whiteColor="#ffff00"/>
             </animator>
          </filter>   
        </overlay> 
      </stack>
  --></div>


.. raw:: html

  <script type="text/javascript" charset="utf-8" src="http://static.stupeflix.com/play/1.2/play-min.js"></script>