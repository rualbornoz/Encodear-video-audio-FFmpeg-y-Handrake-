Encodear audio con FFmpeg
=========================

[Anteriormente vi como desmenuzar el video en Linux](https://web.archive.org/web/20200901184020/https://cont3mpo.github.io/post/2009/edicion-multimedia-con-mencoder-y.html), con MEncoder y MPlayer, pero esta vez hablaré sobre como encodear audio con FFmpeg.

Instalar FFmpeg
---------------

En Linux los encoders nativos y FFmpeg se pueden encontrar en sus repositorios o tienda de apps.

En macOS instala FFmpeg usando [Homebrew](https://web.archive.org/web/20200901184020/http://brew.sh/index_es.html). Y para agregar los codecs que faltan:

`brew reinstall ffmpeg --with-faac --with-opus`

También puedes [descargar un binario de FFmpeg](https://web.archive.org/web/20200901184020/https://evermeet.cx/ffmpeg/) con su última versión para macOS. Instalas en la ruta /usr/local/bin, la primera vez abres el binario manteniendo la tecla alt y click derecho en Abrir para dar permiso de ejecución. Luego puedes usarlo desde Terminal.

Usa una fuente de audio Lossless sin pérdida
--------------------------------------------

Ya teniendo el audio FLAC/ALAC o cualquier [codec lossless](https://web.archive.org/web/20200901184020/https://en.wikipedia.org/wiki/List_of_codecs#Lossless_compression), se puede encodear a un codec de audio [con pérdida](https://web.archive.org/web/20200901184020/https://es.wikipedia.org/wiki/Algoritmo_de_compresi%C3%B3n_con_p%C3%A9rdida#Compresi.C3.B3n_de_audio_con_p.C3.A9rdida) (lossy).

Nunca encodees de un codec lossy con pérdidas a otro lossy, porque quedará peor. Siempre desde un lossless a un lossy.

### Encodea a VBR

La mejor manera de mantener una calidad de audio a través de toda la pista es usando un [bitrate variable VBR](https://web.archive.org/web/20200901184020/https://es.wikipedia.org/wiki/Tasa_de_bits_variable), el cual utiliza solo los bits necesarios para preservar la calidad de audio. Por otro lado, encodear con [bitrate constante CBR](https://web.archive.org/web/20200901184020/https://es.wikipedia.org/wiki/Tasa_de_bits_constante) desperdiciaría bits y sería poco eficiente al otorgar a toda la pista la misma cantidad de bits, encodea a ciegas dando lo mismo a todo.

MP3 (VBR Lossy)
---------------

El codec [MP3](https://web.archive.org/web/20200901184020/https://es.wikipedia.org/wiki/Mp3) se utiliza con el encoder [LAME](https://web.archive.org/web/20200901184020/http://wiki.hydrogenaudio.org/index.php?title=LAME), el cual tiene un rango de calidad entre 0 (mejor) a 9 (peor):

### Lame (VBR)

`lame audio.wav -V 0 audio.mp3`

### FFmpeg MP3 (VBR)

`ffmpeg -i audio.wav -c:a libmp3lame -q:a 0 audio.mp3`

AAC (VBR Lossy)
---------------

El codec [AAC](https://web.archive.org/web/20200901184020/https://es.wikipedia.org/wiki/Advanced_Audio_Coding) se utiliza con el encoder [FAAC](https://web.archive.org/web/20200901184020/https://en.wikipedia.org/wiki/FAAC) con bitrate variable VBR. Funciona con rango de calidad entre 10 (peor) a 500 (mejor):

### Faac (VBR)

`faac audio.wav -q 500 -o audio.aac`

### FFmpeg AAC (VBR)

El encoder nativo aac en FFmpeg tiene un rango de calidad entre 0 (peor) a 2 (mejor):

`ffmpeg -i audio.wav -c:a aac -q:a 2 audio.aac`

Opus (VBR Lossy)
----------------

El codec [Opus](https://web.archive.org/web/20200901184020/https://es.wikipedia.org/wiki/Opus_(c%C3%B3dec)) primero se activa con "vbr on" y un bitrate máximo variable VBR (usaremos 320Kbps):

`ffmpeg -i audio.wav -c:a libopus -vbr on -b:a 320k audio.opus`

DTS (CBR Lossy)
---------------

[DTS](https://web.archive.org/web/20200901184020/https://es.wikipedia.org/wiki/DTS) fue desarrollado por [Xperi](https://web.archive.org/web/20200901184020/https://en.wikipedia.org/wiki/Xperi). Es un codec con pérdida pero es CBR.

`ffmpeg -i audio.wav -c:a dca -strict -2 audio.mka`

FLAC (VBR Lossless)
-------------------

El codec [FLAC](https://web.archive.org/web/20200901184020/https://es.wikipedia.org/wiki/Free_Lossless_Audio_Codec) mantiene un audio sin pérdidas de calidad (Lossless). FLAC soporta hasta [8 canales de audio y 32-bit de profundidad](https://web.archive.org/web/20200901184020/https://xiph.org/flac/faq.html#general__channels):

### Flac (VBR)

`flac audio.wav -o audio.flac`

### FFmpeg FLAC (VBR)

`ffmpeg -i audio.wav -c:a flac audio.flac`

ALAC (VBR Lossless)
-------------------

[ALAC](https://web.archive.org/web/20200901184020/https://es.wikipedia.org/wiki/Apple_Lossless) también mantiene un audio sin pérdidas de calidad (Lossless):

`ffmpeg -i audio.wav -c:a alac audio.m4a`

Extras
------

Para ver información del audio utiliza mediainfo, ffprobe o file:

`mediainfo audio.mp3` `ffprobe audio.mp3` `file audio.mp3`

### Unir audios

Para unir dos archivos de audio se puede realizar lo siguiente:

`cat audio1.mp3 audio2.mp3 > audiopegado.mp3`

En FFmpeg sería algo así:

`ffmpeg -i audio1.mp3 -i audio2.mp3 -c:a copy audiopegado.mp3` 3 de octubre, 2009
