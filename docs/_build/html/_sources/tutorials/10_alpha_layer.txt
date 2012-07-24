.. _alpha_layer:

.. raw:: html

  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

Alpha Layer
===========

Video with an alpha layer
-------------------------

Maybe be you have some video assets that have been keyed, for example shot on a green background then postprocessed to turn the green channel into transparent.
Or maybe you bought some pre-keyed asset on `Toolfarm <https://footage.toolfarm.com/freeClips.asp>`_.
And you want to use them into your Stupeflix creation.

It is now possible, the only constraint is that you have to **turn your video into a flv with alpha layer file**.
To do so, you can use for example Adobe Media Encoder.

In the following example, the Boxing.flv is such an flv with alpha. You can use as usual, except of course that some part of the video maybe transparent.

In the first example, we just use a green background, to show what the transparent video looks like.

.. code-block:: xml

  <stack duration="5">
    <overlay width="1.0" height="1.0" duration="..">
      <image color="#00ff00" />        
    </overlay>  
    <overlay width="1.0" left="0.0" height="1.0">
      <video filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/Boxing.flv" />
    </overlay>
  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack duration="5">
        <overlay width="1.0" height="1.0" duration="..">
          <image color="#00ff00" />        
        </overlay>  
        <overlay width="1.0" left="0.0" height="1.0">
          <video filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/Boxing.flv" />
        </overlay>
      </stack>
  --></div>

In the second one, we use a fancier background.

.. code-block:: xml

  <stack>
    <overlay width="1.0" top="auto" bottom="auto" duration="..">
      <image filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/boxe.jpg" />        
    </overlay>  
    <overlay width="1.0" left="0.0" height="1.0">
      <video filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/Boxing.flv" />
    </overlay>
  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack>    
        <overlay width="1.0" top="auto" bottom="auto" duration="..">
          <image filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/boxe.jpg" />        
        </overlay>  
        <overlay width="1.0" left="0.0" height="1.0">
          <video filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/Boxing.flv" />
        </overlay>
      </stack>
  --></div>

Finally, the limit is your imagination, you can use a video with alpha wherever you can use a video :

.. code-block:: xml

  <stack duration="5">
    <overlay width="1.0" height="1.0" duration="..">
      <image color="#ffffff" />        
    </overlay>  
    <effect type="diving" duration= "10.0">
      <video filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/Boxing.flv" />      
      <image filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/boxe.jpg" />        
    </effect>
  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack duration="5">
        <overlay width="1.0" height="1.0" duration="..">
          <image color="#ffffff" />        
        </overlay>  
        <effect type="diving" duration= "10.0">
          <video filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/Boxing.flv" />      
          <image filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/boxe.jpg" />        
        </effect>
      </stack>
  --></div>

.. code-block:: xml

  <stack duration="5">
    <overlay width="1.0" top="auto" bottom="auto" duration="..">
      <image filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/boxe.jpg" />
      <animator type="slide-in" duration="1.0"/>    
    </overlay>  
    <overlay width="1.0" left="0.0" height="1.0">
      <video filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/Boxing.flv" />
    </overlay>
  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack duration="5">
        <overlay width="1.0" top="auto" bottom="auto" duration="..">
          <image filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/boxe.jpg" />
          <animator type="slide-in" duration="1.0"/>    
        </overlay>  
        <overlay width="1.0" left="0.0" height="1.0">
          <video filename="https://stupeflix-assets.s3.amazonaws.com/code/tutorials/masking/Boxing.flv" />
        </overlay>
      </stack>
  --></div>

.. raw:: html

  <script type="text/javascript" charset="utf-8" src="http://static.stupeflix.com/play/1.2/play-min.js"></script>