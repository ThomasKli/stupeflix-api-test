.. _advanced_text_rendering:

.. raw:: html

  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

Advanced Text Rendering
=======================

Layout
------

If you want to use right to left languages, create different text layout, mix different text styles, the "advanced" text type was introduced just for you.

Sample
++++++

.. code-block:: xml

  <sequence>
    <text type="advanced" fontsize="40" duration="1" reference="AAAAAAA">עִבְרִית </text>
    <text type="advanced" fontsize="40" duration="1" reference="AAAAAAA">العربية </text>
    <text type="advanced" fontsize="40" duration="1" reference="AAAAAAA">汉语/漢語 </text>
    <text type="advanced" fontsize="40" duration="1" reference="AAAAAAA">日本人</text>
    <animator type="custom">
      <key time="0.0" pos="-1,0,0"/>
    </animator>
  </sequence>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <sequence>
        <text type="advanced" fontsize="40" duration="1" reference="AAAAAAA">עִבְרִית </text>
        <text type="advanced" fontsize="40" duration="1" reference="AAAAAAA">العربية </text>
        <text type="advanced" fontsize="40" duration="1" reference="AAAAAAA">汉语/漢語 </text>
        <text type="advanced" fontsize="40" duration="1" reference="AAAAAAA">日本人</text>
        <animator type="custom">
          <key time="0.0" pos="-1,0,0"/>
        </animator>
      </sequence>
  --></div>

Reference text
++++++++++++++

To define the bounding box for the text to be drawn, you just have to specify a reference text. This text will be drawn internally and its bounding box will be used to constrain the placement of the text you use in the "text" xml tag itself.

.. code-block:: xml

  <text type="advanced" duration="1" reference="AAAAAAA">Hello World!</text>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <text type="advanced" duration="1" reference="AAAAAAA">Hello World!</text>
  --></div>

In that first example, you can see that the zone defined by "AAAAAAA" is too small to contain "Hello World", so an ellipsis ("...") is used to truncate it. It is guaranteed that the text won't overflow its bounding box. So, if we set a longer reference text, the ellipsis will disappear.

.. code-block:: xml

  <text type="advanced" duration="1" reference="AAAAAAAAAAAA">Hello World!</text>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <text type="advanced" duration="1" reference="AAAAAAAAAAAA">Hello World!</text>
  --></div>

How can you setup easily the right reference text? The easier is to start with a text tag with no reference text, to check that position and size is ok, and then everything is ok, move it to the "reference" attribute:

.. code-block:: xml

  <text type="advanced" duration="1" >AAAAAAAAAAA</text>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <text type="advanced" duration="1" >AAAAAAAAAAA</text>
  --></div>

then, when the reference text is ok in your layout :

.. code-block:: xml

  <text type="advanced" duration="1" reference="AAAAAAAAAAAA">Hello World!</text>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <text type="advanced" duration="1" reference="AAAAAAAAAAAA">Hello World!</text>
  --></div>

Text wrapping: Multiline Reference text
+++++++++++++++++++++++++++++++++++++++

When you don't add a reference text, there is no bounding box at all. When you add a single lined reference text, an ellipsis will be used if the text is too long. But if you specify a multiple lined reference text, it will wrap, and an ellipsis will be used eventually on the last line.

.. code-block:: xml

  <text type="advanced" duration="1" reference="AAAA&#10;AAAAAA&#10;   AAAAAA">Hello World! This text will be truncated</text>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <text type="advanced" duration="1" reference="AAAA&#10;AAAAAA&#10;   AAAAAA">Hello World! This text will be truncated</text>
  --></div>

The wrapping is done on per word basis first, and on a character per character basis if a single word does not fit on the current line.

Advanced Text wrapping
++++++++++++++++++++++

If you introduce spaces in the reference text, they will be used to shift the line on the right or on the left, depending on the alignment. This can be used to create complex text wrapping.

.. code-block:: xml

  <sequence>
    <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;   AAAAAAAAAAAAAAAAAAAAAAAA&#10;     AAAAAAAAAAAAAAAAAAAAAA&#10;      AAAAAAAAAAAAAAAAAAAAA&#10;      AAAAAAAAAAAAAAAAAAAAA&#10;      AAAAAAAAAAAAAAAAAAAAA&#10;     AAAAAAAAAAAAAAAAAAAAAA&#10;   AAAAAAAAAAAAAAAAAAAAAAAA&#10;AAAAAAAAAAAAAAAAAAAAAAAAAAA">In text display, line wrap is the feature of continuing on a new line when a line is full, such that each line fits in the viewable window, allowing text to be read from top to bottom without any horizontal scrolling.
  Word wrap is the additional feature of most text editors, word processors, and web browsers, of breaking lines between and not within words, except when a single word is longer than a line</text>
    <animator type="custom">
      <key time="0.0" pos="-1.2,0.8,0"/>
    </animator>
  </sequence>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <sequence>
        <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;   AAAAAAAAAAAAAAAAAAAAAAAA&#10;     AAAAAAAAAAAAAAAAAAAAAA&#10;      AAAAAAAAAAAAAAAAAAAAA&#10;      AAAAAAAAAAAAAAAAAAAAA&#10;      AAAAAAAAAAAAAAAAAAAAA&#10;     AAAAAAAAAAAAAAAAAAAAAA&#10;   AAAAAAAAAAAAAAAAAAAAAAAA&#10;AAAAAAAAAAAAAAAAAAAAAAAAAAA">In text display, line wrap is the feature of continuing on a new line when a line is full, such that each line fits in the viewable window, allowing text to be read from top to bottom without any horizontal scrolling.
      Word wrap is the additional feature of most text editors, word processors, and web browsers, of breaking lines between and not within words, except when a single word is longer than a line</text>
      <animator type="custom">
        <key time="0.0" pos="-1.2,0.8,0"/>
      </animator>
      </sequence>
  --></div>

Anchor point
++++++++++++

In the last example, we had to introduce an animator to move the text to be viewable. The reason is that the anchor point for the text is the origin of the first character (including spaces), a point situated on its baseline, on its left for left to right languages (see http://www.freetype.org/freetype2/docs/glyphs/glyphs-3.html for more information).

So, usually, everything will be written under and on the right of this point.

This is true only when the text is left align. If the text is right aligned, the origin will on the right of the last character of the first line. If the text is centered, the origin will be the center of the line.

For right to left language, right/left alignment is inverted. The origin of the text is by default the center of the screen, as you can see on these examples:

.. code-block:: xml

  <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;AAAAAAAAAAAAAAAAAAAAAAAAAA&#10;" align="left">In text display, line wrap is the feature of continuing on a new line.</text>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;AAAAAAAAAAAAAAAAAAAAAAAAAA&#10;" align="left">In text display, line wrap is the feature of continuing on a new line.</text>
  --></div>

.. code-block:: xml

  <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;" align="right">In text display,
   line wrap is the feature of continuing on a new line.</text>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;" align="right">In text display,
         line wrap is the feature of continuing on a new line.</text>
  --></div>

.. code-block:: xml

  <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;" align="center">In text display,
     line wrap is the feature of continuing on a new line.</text>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;AAAAAAAAAAAAAAAAAAAAAAAAAAA&#10;" align="center">In text display,
         line wrap is the feature of continuing on a new line.</text>
  --></div>

.. code-block:: xml

  <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAA&#10;">עִבְרִית</text>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAA&#10;">עִבְרִית</text>
  --></div>

The text that is taken into account for anchoring is actually the reference text, not the the content text. The process is the following:

* the reference text is splitted into lines
* for each line of this reference text, the bounding box and position is computed

then the text itself is drawn

* the align setting is used for each line of text within the bounds of the reference text line
* the text is drawn

Finally, anchoring is performed using the reference text, so the text is shifted to have the center of the screen matching the origin of the text.

.. code-block:: xml

  <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAA&#10;" align="right">עִבְרִית</text>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <text type="advanced" duration="1" reference="AAAAAAAAAAAAAAAAAAAAA&#10;" align="right">עִבְרִית</text>
  --></div>

Style
-----

Example
+++++++

.. code-block:: xml

  <stack>
    <text type="advanced" fontsize="20" duration="1" reference="  AAAAAAAAAAAAA&#10;AAAAAAAAAAAAAAA" weight="bold" style="italic" stretch="condensed" face="times" >In text display, line wrap is the feature of continuing on a new line when a line is full, </text>
    <animator type="custom">  
      <key time="0.0" pos="-1.6,0,0"/>
    </animator>
  </stack>

.. raw:: html

  <div class="ready sxmovie" style="width:640px; height:360px;"><!--
      <stack>
        <text type="advanced" fontsize="20" duration="1" reference="  AAAAAAAAAAAAA&#10;AAAAAAAAAAAAAAA" weight="bold" style="italic" stretch="condensed" face="times" >In text display, line wrap is the feature of continuing on a new line when a line is full, </text>
        <animator type="custom">  
          <key time="0.0" pos="-1.6,0,0"/>
        </animator>
      </stack>
  --></div>

.. raw:: html

  <script type="text/javascript" charset="utf-8" src="http://static.stupeflix.com/play/1.2/play-min.js"></script>