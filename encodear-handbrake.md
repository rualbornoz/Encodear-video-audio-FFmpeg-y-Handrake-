Encodear video/audio con Handbrake
=======================================

### 1080p x264 8-bit (AVC) con CRF

Para encodear con [Handbrake](https://handbrake.fr/) es mucho más fácil, es una app para Windows y macOS que permite encodear rapidamente:

Al cargar el archivo a encodear, elijan Preajuste, y en la sección de Matroska; elijan para video 1080p "H.264 MKV 1080p30". En la pestaña Filtros, todos en Off, apagados.

En la pestaña Video, usa el codificador "H.264 (x264)". En Fotogramas (FPS), usa "Same as source". Esto es importante; en Calidad usa un CRF entre 18 a 23, yo uso 18 (menor número más [preserva calidad](https://web.archive.org/web/20200819170950/https://slhck.info/video/2017/02/24/vbr-settings.html)). En Preajuste de decodificador elegir "Medium". En Perfil del codificador usa "High", y en Nivel de codificación usa "4.1". 

En la pestaña Audio puedes usar una copia directa del audio que tenga al lado "Passthru". En subtitulos deshabilita la opción de "Quemar (Forzar)" para que no se peguen al video, o eliminalos.


### 4K x265 10-bit (HEVC) con CRF

Al cargar el archivo a encodear, elijan Preajuste, y en la sección de Matroska; elijan para 4K "H.265 MKV 2160p60 4K". En la pestaña Filtros, todos en Off, apagados.

En la pestaña Video, usa el codificador "H.265 10-bit (x265)". En Fotogramas (FPS), usa "Same as source". Esto es importante; en Calidad usa un CRF entre 18 a 24, yo uso 18 (menor número más [preserva calidad](https://web.archive.org/web/20200819170950/https://slhck.info/video/2017/02/24/vbr-settings.html)). En Preajuste de decodificador elegir "Medium". En Perfil del codificador usa "Main 10", y en Nivel de codificación usa "5.1". 

En la pestaña Audio puedes usar una copia directa del audio que tenga al lado "Passthru". En subtitulos deshabilita la opción de "Quemar (Forzar)" para que no se peguen al video, o eliminalos.
