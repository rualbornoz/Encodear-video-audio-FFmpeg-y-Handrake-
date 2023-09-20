Encodear con FFmpeg: H.264 (AVC) + FLAC
=======================================

Dejaré de lado a [MEncoder](https://web.archive.org/web/20200901183851/https://es.wikipedia.org/wiki/MEncoder), y usaré [FFmpeg](https://web.archive.org/web/20200901183851/https://es.wikipedia.org/wiki/FFmpeg).

El año 2009 escribí [unas guías sobre edición](https://web.archive.org/web/20200901183851/https://cont3mpo.github.io/post/2009/edicion-multimedia-con-mencoder-y.html) de [video/audio con MEncoder](https://web.archive.org/web/20200901183851/https://cont3mpo.github.io/post/2009/edicion-multimedia-mencoder-y-mplayer.html), es de pelos, pero el proyecto está ahí abandonado, así que ahora (mayo 2013) comenzaré a usar [FFmpeg](https://web.archive.org/web/20200901183851/https://ffmpeg.org/), debido a que actualizan [cada 3 meses](https://web.archive.org/web/20200901183851/https://ffmpeg.org/download.html#releases), es multiplataforma, utiliza [libavcodec](https://web.archive.org/web/20200901183851/http://libav.org/), tiene sintaxis simples y es abierto. Todo esto se puede hacer en [Ubuntu](https://web.archive.org/web/20200901183851/https://cont3mpo.github.io/post/2013/el-futuro-unificado-de-ubuntu.html), [elementary OS](https://web.archive.org/web/20200901183851/https://cont3mpo.github.io/post/2012/probando-elementary-os-luna.html) y [OS X](https://web.archive.org/web/20200901183851/https://cont3mpo.github.io/post/2015/os-x-y-homebrew.html), también [gracias a algunas ayudas](https://web.archive.org/web/20200901183851/http://slhck.info/video-encoding.html) que me topé por ahí.

Medidores de calidad al encodear
--------------------------------

### H.264 (AVC)

Hace ya mucho tiempo que decidí ver todo en [H.264 (AVC)](https://web.archive.org/web/20200901183851/https://es.wikipedia.org/wiki/H.264/MPEG-4_AVC), con su encoder abierto [x264](https://web.archive.org/web/20200901183851/https://es.wikipedia.org/wiki/X264) se logra gran calidad y mejor compresión.

### CRF (Constant Rate Factor)

Fijarse solo en el bitrate al encodear para limitar el peso de un video está mal. La gracia de la opción [CRF](https://web.archive.org/web/20200901183851/http://slhck.info/crf.html) en x264 es que solo se enfoca en mantener una calidad constante al video por medio de distribución de [bitrates](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/Bitrate#Multimedia_encoding) dependiendo de las escenas, él decidirá cuantos bits usar en cada imagen sin importar el peso del video y así mantener una calidad constante

### 10-bit

[10-bit](https://web.archive.org/web/20200901183851/http://wiki.bakabt.me/index.php/Hi10P) brinda una mejor [profundidad de color](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/Color_depth) en un video H.264, el degradado de colores se ve más suave al redimensionar la imagen o encodear desde un blu-ray. 10bit también contribuye en disminuir el peso del video. Actualmente el uso de 10bit se ha popularizado en los encoders de Animes.

### FLAC

La gracia del codec abierto [FLAC](https://web.archive.org/web/20200901183851/https://es.wikipedia.org/wiki/Free_Lossless_Audio_Codec) es que no tiene pérdidas de calidad de audio. Debes usar una fuente de [audio sin pérdidas (Lossless)](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/Blu-ray_Disc#Audio). Usa [LPCM](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/Linear_pulse-code_modulation), [DTS-HD MA](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/DTS-HD_Master_Audio) y [Dolby TrueHD](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/Dolby_TrueHD), los cuales no traen pérdida de audio (lossless), utiliza esos para encodear a FLAC. [DTS](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/DTS_(sound_system)) y [Dolby Digital](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/Dolby_Digital) traen perdida (lossy), así que no encodeen desde esos dos.

Instalar x264 y FFmpeg
----------------------

### Linux x264-10bit

Se instalan 3 cosas, [instalar dependencias (+ yasm), x264, y FFmpeg](https://web.archive.org/web/20200901183851/https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu) (en ese orden), copiar/pegar en la terminal lo que aparece en los cuadros. Para compilar x264-10bit, al final de la linea "./configure" tienes que agregar: --bit-depth=10 (Antes de instalar FFmpeg, primero borra de la lista todos los encoders que no instalaste, como por ejemplo --enable-libfdk-aac, --enable-libmp3lame, etc.)

### Linux x264-8bit

Esto es más fácil, instala [FFmpeg actualizado desde un PPA](https://web.archive.org/web/20200901183851/https://launchpad.net/~mc3man/+archive/ubuntu/trusty-media) (otro [PPA con FFmpeg, la última versión de x264 y Mkvtoolnix](https://web.archive.org/web/20200901183851/https://launchpad.net/~djcj/+archive/ubuntu/hybrid)). Y listo, a encodear.

### macOS x264-8bit

Instala FFmpeg usando [Homebrew](https://web.archive.org/web/20200901183851/http://brew.sh/index_es.html). Y para agregar más encoders:

`brew reinstall ffmpeg --with-faac --with-x265`

### macOS x264-10bit y 8bit

También puedes [descargar un binario de FFmpeg](https://web.archive.org/web/20200901183851/https://evermeet.cx/ffmpeg/) con su última versión para macOS. Instalas en la ruta /usr/local/bin, la primera vez abres el binario manteniendo la tecla alt y click derecho en Abrir para dar permiso de ejecución. Luego puedes usarlo desde Terminal.

Uso básico FFmpeg
-----------------

Si quieres especificar un codec, solamente agrega -c:v (para video) -c:a (para audio), y al lado el nombre del codec correspondiente, en este caso libx264 y flac:

`ffmpeg -i entrada.mkv -c:v libx264 -c:a flac salida.mkv`

Si quieres copiar el video o audio directamente, cambia el nombre de los codecs por copy:

`ffmpeg -i entrada.mkv -c:v copy -c:a copy salida.mkv`

Para ver el nombre de los encoders disponibles usa ffmpeg -encoders y para ver la lista de decoders con ffmpeg -decoders, para ver más opciones de configuración usa ffmpeg --help y la lista de todos los codecs con ffmpeg -codecs

Configurando el encoder
-----------------------

### Usa CRF

CRF tiene un [rango de medición de calidad entre 0 a 63 (en 10bit)](https://web.archive.org/web/20200901183851/https://trac.ffmpeg.org/wiki/Encode/H.264#a1.ChooseaCRFvalue), siendo 0 lo mejor y 63 lo peor. Recomiendo usar 17 para una calidad visual sin pérdidas (lossless). CRF utiliza una sola pasada constante:

`ffmpeg -i entrada.mkv -c:v libx264 -crf 17 -c:a copy salida.mkv`

### Otorgando 10-bit y Compresión

Usando un [Profile](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/Advanced_Video_Coding#Profiles) high10 deja todo listo para los 10-bit, y además un [Level](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/Advanced_Video_Coding#Levels) 5.1.

Para comprimir el archivo se usa un [Preset](https://web.archive.org/web/20200901183851/http://www.chaneru.com/Roku/HLS/X264_Settings.htm#preset), entre más lento el encodeo menor el tamaño del archivo y mejor calidad de imagen. Se puede usar medium (normal por defecto), slow (lento), slower (poco mas lento) o veryslow (muy lento):

`ffmpeg -i entrada.mkv -c:v libx264 -preset medium -crf 17 -profile:v high10 -level 5.1 -c:a copy salida.mkv`

### Compatibilidad con reproductor blu-ray o smart-TV 8-bit

Para compatibilidad elimina los 10-bit, agrega level 4.1, un chroma 4:2:0 y también un profile high, así quedaría "-pix\_fmt yuv420p -level 4.1":

`ffmpeg -i entrada.mkv -c:v libx264 -preset medium -crf 17 -profile:v high -level 4.1 -pix_fmt yuv420p -c:a copy salida.mkv`

Audio FLAC
----------

Para encodear el audio sin perdidas FLAC, se usa así -c:a flac. FLAC soporta [hasta 8 canales de audio y 32 bits de profundidad](https://web.archive.org/web/20200901183851/http://xiph.org/flac/faq.html#general__channels):

`ffmpeg -i entrada.mkv -c:v copy -c:a flac salida.mkv`

Puedes [encodear archivos de audio por separado](https://web.archive.org/web/20200901183851/https://cont3mpo.github.io/post/2009/encodear-audio-en-linux.html).

Las demás opciones las dejé por defecto porque son recomendables, vean más opciones usando ffmpeg --help y x264 --fullhelp.

BDMV (archivos Blu-ray)
-----------------------

Un [BDMV](https://web.archive.org/web/20200901183851/http://tmpgenc.pegasys-inc.com/es/product/taw4_tutorial/taw4_start_bd.html) (común en torrents, [Share](https://web.archive.org/web/20200901183851/http://en.wikipedia.org/wiki/Share_(P2P))/[PerfectDark](https://web.archive.org/web/20200901183851/http://en.wikipedia.org/wiki/Perfect_Dark_(P2P))) incluye todos los archivos del blu-ray, y archivos con contenedor [.m2ts](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/.m2ts), ese archivo contiene el video y las múltiples pistas de audio dentro (PCM, True HD, DTS), usualmente el .m2ts más grande es el video principal.

Para extraer una pista determinada primero tienen que ver que pistas hay dentro.

`ffmpeg -i video.m2ts` `Stream #0:0 Video: AVC   Stream #0:1 Audio: pcm_bluray   Stream #0:2 Audio: ac3`

Por ejemplo 0:0 sería el video a encodear, y 0:1 es la pista de audio lossless que queremos. Se usaría así: -map 0:0 -map 0:1 (y otras opciones recomendadas para encodear blu-ray).

`ffmpeg -i entrada.m2ts -map 0:0 -c:v libx264 -preset medium -crf 15 -profile:v high10 -nal-hrd vbr -b-pyramid strict -slices 4 -aud 1 -map 0:1 -c:a flac salida.mkv`

O también la configuración con opciones nativas recomendadas para encodear blu-ray con el [uso de colores bt709](https://web.archive.org/web/20200901183851/http://mewiki.project357.com/wiki/X264_Settings#colorprim):

`ffmpeg -i entrada.m2ts -map 0:0 -c:v libx264 -preset medium -crf 15 -x264opts nal-hrd=vbr:b-pyramid=strict:slices=4:aud:colorprim=bt709:transfer=bt709:colormatrix=bt709 -map 0:1 -c:a flac salida.mkv`

Opciones extras de FFmpeg
-------------------------

### Resolución video

Para bajar la resolución de imagen de un video (por ejemplo de 1920x1080 a 1280x720) se agrega -vf scale=ancho:alto o también -s:v ancho:alto. Nunca es recomendable subir la resolución, el upscaling queda mal. [Cuidado con las supuestas resoluciones 1080p que no lo son, común en series de anime](https://web.archive.org/web/20200901183851/http://shiroisoranofansub.wordpress.com/2014/03/27/resolucion-anime-y-sus-respectivos-blu-rays/). Pero este método no respetaría el aspecto proporcional en algunos casos (los dos dan el mismo resultado):

`ffmpeg -i entrada.mkv -c:v libx264 -vf scale=1280:720 -c:a copy salida.mkv` `ffmpeg -i entrada.mkv -c:v libx264 -s:v 1280:720 -c:a copy salida.mkv`

Para redimensionar y mantener el aspecto proporcional usa -1 en el alto (no sirve en -s:v):

`ffmpeg -i entrada.mkv -c:v libx264 -vf scale=1280:-1 -c:a copy salida.mkv`

Para redimensionar aleatoriamente y que automáticamente se cree un aspecto proporcional que se ajuste a esa resolución (pero mejor intenta preservar el aspecto original):

`ffmpeg -i entrada.mkv -c:v libx264 -vf scale=800:400,setsar=1:1 -c:a copy salida.mkv`

### Relación aspecto

Mejor conocido como [Aspect Ratio](https://web.archive.org/web/20200901183851/http://www.equasys.de/aspectratio.html), sirve para dar un aspecto proporcional al cuadro de la imagen, como por ejemplo 4:3/1.33 (pantallas de televisores CRT/SD), 3:2/1.5 (cámaras DSLR), 16:10/1.6 (monitor computador), 16:9/1.77 (HDTV), 1.85 (Cinema Film), y 2.35 o 2.39 (Cinemascope), ordenadas desde semicuadrada hasta rectangular. No es recomendable que retoques el aspecto proporcional original de un video, esto solo sirve para casos donde se recorta agresivamente la imagen o casos especiales. El filtro se pone como setdar, por ejemplo 16:9, aunque agregar directamente (sin filtro) -aspect 16:9 también sirve (los dos dan el mismo resultado):

`ffmpeg -i entrada.mkv -c:v libx264 -vf setdar=16:9 -c:a copy salida.mkv` `ffmpeg -i entrada.mkv -c:v libx264 -aspect 16:9 -c:a copy salida.mkv`

### Recortar imagen

Sirve principalmente para recortar bordes negros e imperfecciones en la orilla de un video. El filtro crop=A:B:C:D se explica así: (A) Cortar ancho de la imagen, (B) Cortar alto de la imagen, (C) Mover imagen de derecha a izquierda, (D) Mover imagen de arriba a abajo. El siguiente ejemplo cortaría el alto (B) y movería arriba (D) para centrar un poco:

`ffmpeg -i entrada.mkv -c:v libx264 -vf crop=1280:718:0:2 -c:a copy salida.mkv`

### Dividir y pegar videos

Para dividir partes de un video primero se pone la parte desde donde se extraerá (-ss) y luego el tiempo que se cortará (-t). Por ejemplo si quisieran cortar desde el segundo 30 unos 10 segundos (también se pueden agregar milisegundos para ser más precisos 00:00:10.500):

`ffmpeg -ss 00:00:30 -i entrada.mkv -c:v copy -c:a copy -t 10 salida.mkv`

Para volver a pegar varios videos usualmente sirve concat:

`ffmpeg -i concat:"video1.avi|video2.avi|video3.avi" -c copy pegado.avi`

Pero eso no sirve para los MKV, así que hay que instalar Mvktoolnix ([versión actualizada](https://web.archive.org/web/20200901183851/https://www.bunkus.org/videotools/mkvtoolnix/downloads.html#ubuntu)), el cual trae mkvmerge. Con esto pega los videos en MKV:

`mkvmerge video1.mkv + video2.mkv -o pegado.mkv`

### Extraer video/audio

Para extraer la pista 1 de audio hay que deshabilitar el video (-vn):

`ffmpeg -i entrada.mkv -vn -map 0:1 -c:a copy audio.flac`

Para extraer el video hay que deshabilitar el audio (-an):

`ffmpeg -i entrada.mkv -c:v copy -an video.mkv`

Para extraer solo el video sin que copie el subtitulo flotante (-sn):

`ffmpeg -i entrada.mkv -c:v copy -an -sn video.mkv`

Si quisiéramos extraer el video junto con una pista de audio determinada, por ejemplo el Stream #6 de audio (con "ffmpeg -i video.mkv" se puede saber el número de Stream):

`ffmpeg -i entrada.mkv -map 0:0 -c:v copy -map 0:6 -c:a copy salida.mkv`

### Extraer con Mkvextract

Con mkvextract (incluido en mkvtoolnix) también se pueden extraer las pistas de video, audio y subtitulos (con mkvinfo se puede ver el número ID de cada pista):

`mkvextract tracks video.mkv 0:video.mkv 1:audio.aac 2:subtitulo.srt`

### Unir video/audio y agregar más pistas de audio

Para unir video (sin audio) y el nuevo audio, en MKV se demora:

`ffmpeg -i video.mkv -i audio.flac -c:v copy -c:a copy unido.mkv`

Mkvmerge es ideal para adjuntar archivos a MKV, agrega el audio a la última pista, y es rápido:

`mkvmerge video.mkv audio.flac -o unido.mkv`

Para agregar múltiples pistas de audio y subtitulo:

`mkvmerge video.mkv audio1.flac audio2.ac3 sub.srt -o unido.mkv`

### Agregar información video/audio

Después de agregar todas las pistas, se puede agregar información de idioma o título a las pistas, se usa --language id:lengua ("mkvmerge --list-languages" para ver la lista) y --track-name id:"Información". Con "mkvmerge -i video.mkv" se puede ver el ID de cada pista.

`mkvmerge --language 1:eng --track-name 1:"Inglés FLAC 5.1" --language 2:spa --track-name 2:"Español AC3 5.1" --lenguage 3:spa --track-name 3:"Español SubRip" entrada.mkv -o salida.mkv`

### Poner bitrate

Si quieres un peso determinado o limitar los bits usa el [bitrate](https://web.archive.org/web/20200901183851/https://es.wikipedia.org/wiki/Bitrate), eso se puede ajustar con -b:v, adjuntando el numero del bitrate con una k de kBits o m de mBits al lado, los dos ejemplos brindan el mismo resultado, aunque k es más preciso:

`ffmpeg -i entrada.mkv -c:v libx264 -b:v 4000k -c:a copy salida.mkv` `ffmpeg -i entrada.mkv -c:v libx264 -b:v 4m -c:a copy salida.mkv`

Para audio agrega -b:a:

`ffmpeg -i entrada.mkv -c:v copy -c:a libfaac -b:a 640k salida.mkv`

### FPS

Si queremos bajar los [FPS](https://web.archive.org/web/20200901183851/https://es.wikipedia.org/wiki/Im%C3%A1genes_por_segundo), por ejemplo a [24 (23.976)](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/24p), se agrega -vf fps= (los dos ejemplos dan el mismo resultado):

`ffmpeg -i entrada.mkv -c:v libx264 -vf fps=23.976 salida.mkv` `ffmpeg -i entrada.mkv -c:v libx264 -vf fps=24000/1001 salida.mkv`

O entero pero sin subframes con -r:

`ffmpeg -i entrada.mkv -c:v libx264 -r 24 salida.mkv`

### Grabar escritorio, audio micrófono/cámara

Para grabar todo el escritorio en video de alta calidad (salir: Q):

`ffmpeg -f x11grab -s 1440x900 -framerate 30 -i :0.0 -c:v libx264 -qp 0 -preset ultrafast salida.mkv`

Grabar audio desde el micrófono del notebook. O grabar video+audio desde la cámara del notebook (salir: Ctrl + C). Vean en alsamixer si tienen el micrófono en captura al 100 (Cambiar a captura alsamixer: TAB):

`ffmpeg -f alsa -i pulse salida.wav` `ffmpeg -f video4linux2 -i /dev/video0 -f alsa -i pulse salida.mkv`

Ayudas
------

Y varios [filtros de ffmpeg para modificar video/audio](https://web.archive.org/web/20200901183851/https://ffmpeg.org/ffmpeg-filters.html) (úsalos responsablemente). También hace tiempo escribí una guía sobre como [encodear audio por separado](https://web.archive.org/web/20200901183851/http://cont3mpo.github.io/post/2009/encodear-audio-en-linux.html). Para más ayuda vean la [documentación completa de FFmpeg](https://web.archive.org/web/20200901183851/http://ffmpeg.org/documentation.html). También más cosas en [la wiki para encodeo de x264](https://web.archive.org/web/20200901183851/https://ffmpeg.org/trac/ffmpeg/wiki/x264EncodingGuide).

Algunas [ayudas extras](https://web.archive.org/web/20200901183851/http://slhck.info/video-encoding.html) no oficiales que me [topé por ahí](https://web.archive.org/web/20200901183851/http://sonnati.wordpress.com/2011/08/08/ffmpeg-%E2%80%93-the-swiss-army-knife-of-internet-streaming-%E2%80%93-part-ii/). Un documento de x264 sobre las [diferencias técnicas de 8bit y 10bit](https://web.archive.org/web/20200901183851/http://x264.nl/x264/10bit_02-ateme-why_does_10bit_save_bandwidth.pdf). Unas guías de la wiki para [encodear diferentes cosas](https://web.archive.org/web/20200901183851/https://ffmpeg.org/trac/ffmpeg/wiki#Encoding). La lista de [opciones disponibles para x264 y su función](https://web.archive.org/web/20200901183851/http://www.chaneru.com/Roku/HLS/X264_Settings.htm).

Asegúrate de instalar el [reproductor mpv](https://web.archive.org/web/20200901183851/https://cont3mpo.github.io/post/2013/mpv-el-sucesor-de-mplayer2.html). Utiliza [Screenshot Comparison](https://web.archive.org/web/20200901183851/http://screenshotcomparison.com/) para subir capturas de video y comparar diferencias de imagen con tus encodeos. Utiliza [Mediainfo](https://web.archive.org/web/20200901183851/http://mediainfo.sourceforge.net/es) para ver en detalle la información del video/audio (con vista HTML o por terminal es más detallado).

18 de mayo, 2013
