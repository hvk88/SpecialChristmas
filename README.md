# SpecialChristmas
COOPDELUXE SPECIAL GAME


# Modos con sonido

Este código en Lua gestiona la reproducción de archivos de audio mediante un sistema personalizado. Vamos a desglosar las secciones del script para que entiendas cada línea y su propósito.


---

Sección Inicial: Declaración de Variables
```
audioStream = nil;
audioSample = nil;
```
audioStream: Se utiliza para manejar un flujo de audio, en este caso, el archivo music.mp3.

audioSample: Se utiliza para manejar un sample o muestra de audio, en este caso, sample.mp3.

Ambas variables están inicializadas como nil porque no contienen ningún dato hasta que se carguen archivos de audio.



---

Función on_stream_play(msg)

Esta función se encarga de manejar los comandos relacionados con el flujo de audio (audioStream). El parámetro msg contiene el comando recibido.

1. Cargar el audio (load)
```
if(msg == "load") then
    audioStream = audio_stream_load("music.mp3")
    audio_stream_set_looping(audioStream, true)
    djui_chat_message_create("audio audioStream:" .. tostring(audioStream));
end
```
audio_stream_load("music.mp3"): Carga el archivo music.mp3 en memoria y lo asigna a audioStream.

audio_stream_set_looping(audioStream, true): Configura el audio para que se repita en bucle una vez que comience a reproducirse.

djui_chat_message_create: Muestra un mensaje en el chat indicando que el audio se ha cargado y el identificador de audioStream.



---

2. Reproducir el audio (play)
```
if(msg == "play") then
    audio_stream_play(audioStream, true, 1);
    djui_chat_message_create("playing audio");
end
```
audio_stream_play(audioStream, true, 1): Reproduce audioStream desde el principio. Los parámetros indican:

true: Indica que se reproducirá desde el inicio (ignora la posición actual).

1: Es el volumen de reproducción.


Muestra un mensaje en el chat: "playing audio".



---

3. Reanudar el audio (resume)
```
if(msg == "resume") then
    audio_stream_play(audioStream, false, 1);
    djui_chat_message_create("resuming audio");
end
```
Igual que play, pero el parámetro false indica que se reanuda desde la posición actual del audio.



---

4. Pausar el audio (pause)
```
if(msg == "pause") then
    audio_stream_pause(audioStream);
    djui_chat_message_create("pausing audio");
end
```
audio_stream_pause(audioStream): Pausa la reproducción del audio actual.



---

5. Detener el audio (stop)
```
if(msg == "stop") then
    audio_stream_stop(audioStream);
    djui_chat_message_create("stopping audio");
end
```
audio_stream_stop(audioStream): Detiene por completo la reproducción y resetea la posición al inicio.



---

6. Destruir el audio (destroy)
```
if(msg == "destroy") then
    audio_stream_destroy(audioStream);
    djui_chat_message_create("destroyed audio");
end
```
audio_stream_destroy(audioStream): Libera la memoria asignada al flujo de audio.



---

7. Obtener la posición del audio (getpos)
```
if(msg == "getpos") then
    djui_chat_message_create("pos: " .. tostring(audio_stream_get_position(audioStream)));
end
```
audio_stream_get_position(audioStream): Obtiene la posición actual de reproducción en segundos.



---
```
Función on_sample_play(msg)
```
Esta función maneja los comandos relacionados con el audio de muestra (audioSample).

1. Cargar el sample (load)
```
if(msg == "load") then
    audioSample = audio_sample_load("sample.mp3");
    djui_chat_message_create("audio audioStream:" .. tostring(audioSample));
    return true;
end
```
audio_sample_load("sample.mp3"): Carga el archivo sample.mp3 en memoria y lo asigna a audioSample.

Muestra un mensaje indicando que el audio se ha cargado.



---

2. Reproducir el sample (play)

audio_sample_play(audioSample, gMarioStates[0].pos, 1);

audio_sample_play(audioSample, gMarioStates[0].pos, 1):

Reproduce el sample audioSample.

gMarioStates[0].pos: Define la posición espacial del sonido (usado en sistemas 3D).

1: Define el volumen.




---

Hooks de Comandos

Al final, el script registra los comandos que activan las funciones anteriores:

hook_chat_command('stream', "[load|play|resume|pause|stop|destroy|getpos]", on_stream_play)
hook_chat_command('sample', "[load|play]", on_sample_play)

hook_chat_command: Registra un comando del chat.

'stream': Comando que activa la función on_stream_play.

'sample': Comando que activa la función on_sample_play.

Los valores entre corchetes son los subcomandos aceptados.




---

Resumen

Este script:

1. Carga y reproduce audio tanto en formato de flujo como en muestras individuales.


2. Ofrece control completo (play, pause, stop, etc.) sobre el audio cargado.


3. Responde a comandos del chat para facilitar el control de la música y sonidos.
