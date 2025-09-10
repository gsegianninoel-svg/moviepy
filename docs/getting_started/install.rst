Durée des clips
clips = [
    {"text": "Bébé mais déjà une star ✨", "color": (255,223,186), "duration":7},
    {"text": "L'ado en pleine crise 😂", "color": (186,255,201), "duration":8},
    {"text": "Toujours prête à faire la fête 🍾", "color": (186,225,255), "duration":10},
    {"text": "Happy 18 🎂🥂", "color": (255,186,255), "duration":10}
]

video_clips = []

for clip in clips:
    # Fond coloré (à remplacer par une image réelle si souhaité)
    c = ColorClip(size=(720,1280), color=clip["color"], duration=clip["duration"])

Zoom léger
    c = c.resize(lambda t: 1 + 0.05*t/clip["duration"])

Texte animé (fadein)
    txt = TextClip(clip["text"], fontsize=60, color='black', font='Amiri-Bold')
    txt = txt.set_position('center').set_duration(clip["duration"]).fadein(1)

Composition
    comp = CompositeVideoClip([c, txt])

Transition fondu
    comp = comp.fadein(1).fadeout(1)

    video_clips.append(comp)

Combiner les clips
final_video = concatenate_videoclips(video_clips, method="compose")

Ajouter musique (optionnel)
audio = AudioFileClip("musique_libre.mp3").subclip(0, final_video.duration)
final_video = final_video.set_audio(audio)
Exporter la vidéo MP4 verticale TikTok
final_video.write_videofile("mini_montage_18ans_dynamic.mp4", fps=24)
.. _install:

Installation
============

Installation is done with ``pip``. If you don't have ``pip``, take a look at `how to install it <https://pip.pypa.io/en/stable/installation/>`_.

With ``pip`` installed, just type this in a terminal:

.. code:: bash

    $ (sudo) pip install moviepy

.. _install-binaries:

Installation of Additional Binaries
-----------------------------------

MoviePy depends on the software ffmpeg_ for video reading and writing and on ``ffplay`` for video previewing.

You don't need to worry about ffmpeg_, as it should be automatically downloaded/installed by ImageIO during your first use of MoviePy (it takes a few seconds).

You do need to worry about ``ffplay`` if you plan on using video/audio previewing. For these cases, make sure to have ``ffplay`` installed (it can usually be found alongside ``ffmpeg``) and ensure it is accessible to Python, or define a custom path (see below).

Define Custom Paths to Binaries
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to use a specific version of FFmpeg and FFplay, you can do so using environment variables.

There are a couple of environment variables used by MoviePy that allow you to configure custom paths to the external tools.

To set up any of these variables, the easiest way is to do it in Python before importing objects from MoviePy. For example:

.. code-block:: python

    import os
    os.environ["FFMPEG_BINARY"] = "/path/to/custom/ffmpeg"
    os.environ["FFPLAY_BINARY"] = "/path/to/custom/ffplay"

Alternatively, after installing the optional dependencies, you can create
a ``.env`` file in your working directory that will be automatically read.
For example

.. code-block:: ini

    FFMPEG_BINARY=/path/to/custom/ffmpeg
    FFPLAY_BINARY=/path/to/custom/ffplay

Environment Variables
---------------------

There are two available environment variables for external binaries:

``FFMPEG_BINARY``
    Normally you can leave it at its default ('ffmpeg-imageio'), in which
    case imageio will download the correct ffmpeg binary (on first use) and then always use that binary.

    The second option is ``"auto-detect"``. In this case, ffmpeg will be whatever
    binary is found on the computer: generally ``ffmpeg`` (on Linux/macOS) or ``ffmpeg.exe`` (on Windows).

    Lastly, you can set it to use a binary at a specific location on your disk by specifying the exact path.

``FFPLAY_BINARY``
    The default is ``"auto-detect"``. MoviePy will try to find and use the installed ``ffplay`` binary.

    You can set it to use a binary at a specific location on your disk. On Windows, this might look like:

    .. code-block:: python

        os.environ["FFPLAY_BINARY"] = r"C:\Program Files\ffmpeg\ffplay.exe"

Verify if MoviePy Finds Binaries
--------------------------------

To test if FFmpeg and FFplay are found by MoviePy, in a Python console, you can run:

.. code:: python

    from moviepy.config import check
    check()

.. _ffmpeg: https://www.ffmpeg.org/download.html
