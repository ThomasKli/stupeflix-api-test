.. _stupeflix_xml_langage:

.. raw:: html

  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

Stupeflix XML Language
======================

Introduction
------------

The Stupeflix Video Description Language is an XML language (called SXML below).

It is intended to be as simple as possible to create videos, yet powerful enough to allow complex projects. It was designed around several **fundamental ideas**:

* it is XML, and fully human readable
* it is 4D
* it is vector-based

XML, and fully human readable
+++++++++++++++++++++++++++++

A lot of software claims that they are able to export XML documents. Sometimes it is true and sometimes the XML contains actually chunk of binary data, in hexadecimal form, if you are lucky. That's not the most useful form of XML most developers would say.

So even if in the end most of our customers use scripts to generate SXML bits, the XML is always human readable. So, you can understand and debug it easily. You can share snippets of XML to show how to create a nice effect you designed. You can use all the standard tools to create XML or to process it. XSLT stylesheets are for example a good way to build video templates that will transform your data into a SXML file.

The description hierarchy is a good fit with the XML node hierarchy, and I hope you will find it natural to use. I do, but you know, I am not really that objective on this subject.

And, finally, you can even decide to rewrite a video rendering engine using the same XML language of you want. Don't forget to contact us if you do so!

More seriously, your projects will have some chance to be used in another place, in another time, outside the Stupeflix platform.

4D
++

Everything is 3D in SXML, even 2D elements. And even more, everything is 4D, as everything can be animated.

So, if you decide to animate your scene, your 2D elements will in fact reveal as 3D planes, just as every other ones. In a similar way, if you decide to alter filters parameters throughout a sequence, just add an animator as child element in the XML. Each node in the hierarchy can be animated: think of the SXML as a large scene graph, so if you animate one node, all sub-elements will move in 3D.

Vector based video description
++++++++++++++++++++++++++++++

Vector based graphics are well known. That means that you can scale up them without a loss of precision. In the SXML language, the fundamental idea is the same. However, the idea is pushed a little bit further.

The same SXML file can be used to generate different videos, at different resolution. That's nice, that's exactly the kind of things Vector based graphics are good at. BUT, an issue quickly emerges: what about aspect ratio? How do you render the video with a 16:9 ratio for a plasma screen, a 4:3 ratio for a web video, or 3:2 ratio for an iPhone? Do you stitch the video? It would be ugly.

The answer is that everything in the language should be liquid to adapt to the video aspect ratio, much more a web page than classical vector graphics. When you position your overlays, you specify margins, just like in CSS. When you add images in an effect, their aspect ratio is kept. When you use animators, you can ignore the screen ratio and just say "slide-in" or "slide-out".

But maybe the most important concept is about time: you won't find any timeline in the SXML. Instead, you can nest video sequence in the language itself, and container duration will be computed from its content. For example, you want to create videos using a fixed template and with some voice over. The voice over duration is different each time. In a standard timeline based video editor, you would have to move around video elements to adapt to the new audio duration. With the SXML, just includes the voice over, its duration will be detected before rendering starts, and propagated throughout the full video description. You can even download the XML with every node annotated with duration information, just by using a special API call, for debugging purpose for example.

You will see throughout the upcoming tutorials other examples of this important concept.

Conclusion
++++++++++

So, that's all for today. Next time, we will talk less about philosophy, and more about action!


.. raw:: html

  <script type="text/javascript" charset="utf-8" src="http://static.stupeflix.com/play/1.2/play-min.js"></script>