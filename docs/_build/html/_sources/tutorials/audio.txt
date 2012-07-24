.. _audio:

.. raw:: html

  <link rel="stylesheet" href="http://static.stupeflix.com/play/1.2/style-min.css" type="text/css" charset="utf-8"/>

Audio, Voice Over & Text-To-Speech
==================================

Audio
-----

What would be video without audio ? Fortunately, handling multiple audio tracks is built-in the SXML.

So what can you do ?

* integrate an arbitrary number of audio tracks in the stack-sequence hierarchy
* superpose several audio tracks at the same time
* adjust volume
* add fade-in / fade-out effects
* skip the first n seconds in the audio track

In addition, you can also produce audio tracks, using speech synthesis. Finally, by default, every video you add in an effect or an overlay come with its own audio track, which is mixed by default with the other audio tracks.

Basic use, and a common mistake
+++++++++++++++++++++++++++++++

You may want to add an audio track to your video using something similar to:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <effect type="diving">
        <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
        <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
        <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Lightning_over_Oradea_Romania_3.jpg"/>
      </effect>
      <audio filename="http://assets.stupeflix.com/code/tutorials/audio/soundtracks/Flypaper.mp3"/>
    </body>
  </movie>

But this is not the way it works : just see the result at `audio_sequence.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_sequence.mp4>`_.

The "body" xml node is a sequence, and so the audio will be played after the effect finishes, not at the same time. The effect will use its default duration, 5 seconds.

So the correct way to do so is to add a stack to your xml and place your effect and your audio in it.

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack>
        <effect type="diving">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
        </effect>        
        <audio filename="http://assets.stupeflix.com/code/tutorials/audio/soundtracks/Flypaper.mp3"/>
      </stack>
    </body>
  </movie>

Just remember this point, and using audio will be really easy.

Here is the result: `audio_fullduration.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_fullduration.mp4>`_.

As you can see, the total duration of the video is 3:20, the duration of the music.

Is this really what we wanted ? Not really. So, let see how to change this.

Multiple tracks: audio as slave
+++++++++++++++++++++++++++++++

So how can you add several audio at the same time, and how do you force them to adopt the length of the rest of video elements ? Just use the xml structure to do so:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <!-- All music by Josh Woodward, The Simple Life -->
      <stack>
        <effect type="diving" duration="10">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
        </effect>        
        <audio filename="http://assets.stupeflix.com/code/tutorials/audio/soundtracks/Flypaper.mp3" duration=".." fadein="2"/>
      </stack>
      <transition type="swirl" duration="2.0"/>
      <stack>
        <effect type="square" duration="15">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
        </effect>
        <audio filename="http://assets.stupeflix.com/code/tutorials/audio/soundtracks/ImLettingGo.mp3" duration=".." fadeout="2"/>
      </stack>
    </body>
  </movie>

You can find the result here : `audio_slave.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_slave.mp4>`_.

This way, the audio is bound to play with the effect.

Note the ``duration=".."`` directive in the audio tags: it bounds the duration of the audio track to its parent xml node duration. The stack will look for its children to determine its duration, and so it will ignore the audio tag, as this latter rely itself on its parent. So, that's the effect node that fix the duration. If you remove this directive, the effect and the stack node duration will be fixed by the audio duration.

You can see some sample use of fadein and fadeout directives.

Note that the transition has an effect on audio too : it creates automatically a crossfade between the two soundtracks.

Multiple tracks: audio as master
++++++++++++++++++++++++++++++++

So, adding a soundtrack is easy. But what about adding a voiceover when you do not know how long it is because it's created using text to speech ? You can play with durations the other way round:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <!-- All music by Josh Woodward, The Simple Life -->
      <stack>
        <effect type="diving">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
        </effect>        
        <audio filename="http://assets.stupeflix.com/code/tutorials/audio/soundtracks/Flypaper.mp3" duration=".." fadein="2" fadeout="2" volume="0.1" skip="5"/>
        <audio voice="neospeech:julie" margin-end="2.0">Music by Josh Woodward, Flypaper, from the album The Simple Life. Available on Jamendo dot com. Images from Wikipedia Picture of the Day.</audio>
      </stack>
    </body>
  </movie>

Here is the result: `audio_master.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_master.mp4>`_.

In that sample, the music duration is set to ".." , as previously. But we removed the duration from the effect. So, what is the duration of the stack, effect, and music ? It's the duration of the last audio track, the voice over. Duration is obtained from the audio file produced by the speech synthetizer. Note the margin-end added to the voice over, to allow some extra time at the end of the voice over and before the end of the video. You can add the same way a "margin-start" directive if you want. Finally, we used the volume directive on the music, to put the emphasis on the voice rather than on the music, and we skipped the first 5 seconds of the music.

Audio sequences
+++++++++++++++

You can easily concatenate different audio tracks. Just create as sequence of audio files. Here we use speech synthesis, but it is exactly the same with standard audio files.

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack>
        <effect type="diving" duration="10">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Hopetoun_falls.jpg"/>
        </effect>        
        <sequence>
           <audio voice="neospeech:julie">Hello.</audio>
           <audio voice="neospeech:paul">Who are you ?</audio>
           <audio voice="neospeech:julie">I am Julie. Don't you remember ?</audio>
           <audio voice="neospeech:paul">No, definitely not.</audio>
        </sequence>
      </stack>
    </body>
  </movie>

Here is the result: `audio_sequence.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_sequence.mp4>`_.

Using video with audio
++++++++++++++++++++++

Remember, when you add a video to an effect or to an overlay, an audio track is automatically created. You can prevent the audio track to be created by specifying audio="false" in the video node.

Let's see some examples:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <effect type="diving" duration="10">
        <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
        <video filename="http://assets.stupeflix.com/code/tutorials/audio/video/sts120_launch.mov"/>
      </effect>
    </body>
  </movie>

The result is here : `audio_video.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_video.mp4>`_.

How does it works ? The diving effect has a specified duration, so it imposes it to its children. The video will play after roughly 5 seconds, for 5 seconds, and so will do the related audio track.

Now, let's remove the audio:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <effect type="diving" duration="10">
        <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
        <video filename="http://assets.stupeflix.com/code/tutorials/audio/video/sts120_launch.mov" audio="false"/>
      </effect>
    </body>
  </movie>

See it in action : `audio_video_noaudio.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_video_noaudio.mp4>`_.

Finally, let's add an audio track from a video:

.. code-block:: xml

  <movie service="craftsman-1.0">
    <body>
      <stack>
        <audio filename="http://assets.stupeflix.com/code/tutorials/audio/video/sts120_launch.mov" fadein="2" fadeout="2" duration=".."/>
        <effect type="diving" duration="10">
          <image filename="http://assets.stupeflix.com/code/homevideo/images/wiki_Double-O-Arch_Arches_National_Park_2.jpg"/>
          <video filename="http://assets.stupeflix.com/code/tutorials/audio/video/sts120_launch.mov" audio="false"/>
        </effect>        
      </stack>
    </body>
  </movie>

Remember, we have to add a stack to be able to superpose the audio with the effect. Then, we have to add ``duration=".."`` to the audio, so it is a slave of its "stack" parent. And finally, we set ``audio="false"`` on the video.

See it now: `audio_video_asaudio.mp4 <http://assets.stupeflix.com/code/tutorials/audio/audio_video_asaudio.mp4>`_.

That was actually a good recapitulation of what we saw earlier today.

You said easy? I heard you!

.. raw:: html

  <script type="text/javascript" charset="utf-8" src="http://static.stupeflix.com/play/1.2/play-min.js"></script>