.. _masking:

.. raw:: html

  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

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

.. raw:: html

  <script type="text/javascript" charset="utf-8" src="http://static.stupeflix.com/play/1.2/play-min.js"></script>