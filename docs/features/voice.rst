Voice
=====

Very often in an adventure game you want your protagonist to say
something, e.g. to comment a specific behavior or event. GOAT makes that
process easier for you.

First thing you need to do is to register a voice sample:

::

   goat_voice.register(audio_name, transcript)

All voice samples must be stored in the ``voice`` directory of your game
resources folder. Currenly, only OGG files are supported. ``audio_name``
must match the name of an audio file in the ``voice`` directory (e.g.
``demo/voice/but_why.ogg`` becomes ``but_why``). ``transcript`` is the
text included in the voice recording (in this case, probably, "But
why?"). It will be shown at the bottom of the screen when the recording
is played. The transcript is fully translatable (you just need to add the
translation to your Godot project).

There is an additional parameter to this method: ``time``. If you specify a
value greater than 0, the ``register`` method will not attempt to load the
audio file. Instead, it will register the transcript and the duration it should
be "played" (for the purpose of showing the subtitles). This feature is useful
if you don't want to or cannot provide an audio sample, but still want to see
the subtitles.

|Subtitles shown when the voice is playing|

You can play a registered voice sample at any time:

::

   goat_voice.play(audio_names)

``audio_names`` can be either a single name or an array of correct audio
names (in which case a random one will be played). To stop the current voice
recording, call:

::

   goat_voice.stop()

There are 2 signals associated with voice recordings:

::

   goat_voice.started (audio_name)
   goat_voice.finished (audio_name, interrupted)

If a voice recording is already playing while a new one starts, the old
one will be stopped first and ``goat_voice.finished`` signal will be
emitted (with ``interrupted`` set to ``true``). Otherwise the signal
will be emitted after the recording is played fully (with
``interrupted`` set to ``false``).

Transcript for any recording can be obtained this way:

::

   goat_voice.get_transcript(audio_name)

If you want to clear all registered voice recordings, you can do it like
this:

::

   goat_voice.reset()

Default voice recordings
------------------------

In GOAT you can set a group of default voice recordings, that will be
played when certain actions happen. For example, you can configure lines
like "But why?" or "What for?" to be played when the player tries to use
items in a way that doesn't make sense.

You can set default voice recordings like this (remember to register
them first):

::

   goat_voice.set_default_audio_names([audio_name1, audio_name2, ...])

You can also play a random default recording later:

::

   goat_voice.play_default()

You might want to play a random recording every time a meaningless
action happens, e.g. when the player uses an item that is not supposed
to be used. You can automate this process by connecting default
recordings to signals:

::

   goat_voice.connect_default(object, signal_name)

This way a random recording will be played when a signal is emitted by
an object. For example, to play a default recording every time the
player activates an object, you can use this code:

::

   goat_voice.connect_default(goat_interaction, "object_activated")

However, if the player performs a meaningful action, you probably want
do play a specific voice recording and ignore the default ones. In that
case, you can deactivate the default recordings (just for one action)
like this:

::

   goat_voice.prevent_default()

.. |Subtitles shown when the voice is playing| image:: https://user-images.githubusercontent.com/36821133/73210781-425d8800-414b-11ea-9a7e-0c0527c0e47d.png
