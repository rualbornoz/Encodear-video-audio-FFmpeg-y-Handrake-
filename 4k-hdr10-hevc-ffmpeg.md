Encodear con FFmpeg: 4K HDR10 HEVC (x265)
==================================

Últimamente se han lanzado muchos blu-ray [4K](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/Ultra-high-definition_television) con [HDR10](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/High-dynamic-range_video#HDR10), y volví a [FFmpeg](https://web.archive.org/web/20200901183851/https://cont3mpo.github.io/post/2013/encodear-con-ffmpeg.html) para probar esto.

Conceptos de video de última generación
---------------------------------------

### HDR10

Esto es simplemente los 10-bit de profundidad de color que vienen en los blu-ray 4K, pero con [HDR](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/High-dynamic-range_video) (Rango Alto Dinámico) habilitado.

### 4K

La resolución máxima 3840×2160 de los blu-ray 4K de ahora, e [incluso Netflix](https://web.archive.org/web/20200901183851/https://netflixtechblog.com/bringing-4k-and-hdr-to-anime-at-netflix-with-sol-levante-fa68105067cd) y [Apple TV+](https://web.archive.org/web/20200901183851/https://support.apple.com/en-us/HT207949) proveen esta resolución. No tocaremos esa resolución.

### HEVC (x265)

El nuevo codec de alta eficiencia en calidad y peso (ya había publicado como [usar HEVC con FFmpeg](https://web.archive.org/web/20200901183851/https://cont3mpo.github.io/post/2015/encodear-con-ffmpeg-hevc-h265.html)). Usaremos la libreria [x265](https://web.archive.org/web/20200901183851/https://x265.readthedocs.io/en/default/index.html) para encodear.

### BT.2020

Con [BT.2020](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/Rec._2020) se define una amplia gama de colores y sus usos en video para 4K, anteriormente se usaba BT.709 en los blu-ray 2K.

Instalar FFmpeg
---------------

En macOS instala FFmpeg usando [Homebrew](https://web.archive.org/web/20200901183851/http://brew.sh/index_es.html). Y para instalar los encoders que faltan:

`brew reinstall ffmpeg --with-faac --with-x265`

También puedes [descargar un binario de FFmpeg](https://web.archive.org/web/20200901183851/https://evermeet.cx/ffmpeg/) con su última versión para macOS. Instalas en la ruta /usr/local/bin, la primera vez abres el binario manteniendo la tecla alt y click derecho en Abrir para dar permiso de ejecución. Luego puedes usarlo desde Terminal.

En Linux puedes instalar desde la tienda de apps o repositorios.

Configurando el encoder
-----------------------

### Habilitar 10-bit y CRF

Para esto hay que habilitar HEVC 10-bit, y eso se hace con un [Profile](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding#Profiles) main10 junto con un [Level](https://web.archive.org/web/20200901183851/https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding#Tiers_and_levels) 5.1. 

La opción CRF en x265 tiene un rango entre 18 a 24 (menor número más [preserva calidad](https://web.archive.org/web/20200819170950/https://slhck.info/video/2017/02/24/vbr-settings.html)). Recomiendo usar 18 para una calidad visual sin pérdidas. 

### Compresión

Para comprimir se usa [Preset](https://web.archive.org/web/20200901183851/https://x265.readthedocs.io/en/default/presets.html#presets), este comprime el tamaño del archivo, entre más lento el encodeo menor el tamaño del archivo, usaremos medium (por defecto es profile automático, preset medium y level automático).

### HDR

Y para habilitar el contenido HDR necesitamos activar las opciones de x265 en x265-params, que son [hdr10](https://web.archive.org/web/20200901183851/https://x265.readthedocs.io/en/default/cli.html#cmdoption-hdr10) y [hdr10-opt](https://web.archive.org/web/20200901183851/https://x265.readthedocs.io/en/default/cli.html#cmdoption-hdr10-opt). Al igual que activar todas las opciones de bt2020. Y por último, también activar al inicio un chroma 4:2:0 para 10-bit, que es pix\_fmt yuv420p10le.

`ffmpeg -i entrada.mkv -pix_fmt yuv420p10le -max_muxing_queue_size 9999 -c:v libx265 -crf 18 -profile:v main10 -x265-params level=5.1:hdr10=1:hdr10-opt=1::no-sao=1:aq-mode=3:max-merge=4:keyint=60:bframes=3:repeat-headers=1:colorprim=bt2020:transfer=smpte2084:colormatrix=bt2020nc:master-display="G(13250,34500)B(7500,3000)R(34000,16000)WP(15635,16450)L(10000000,500)" -c:a copy salida.mkv`

Este encodeo es compatible con un televisor 4K HDR que lea archivos mkv (Sony, LG y Samsung ya leen archivos con HDR).

19 de junio, 2020
