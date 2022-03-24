2022-03-23 16:03
#tag
# youtube
### Download
#### [[pytube]]
```python
from pytube import YouTube


yt = YouTube('https://www.youtube.com/watch?v=ldNTlFebTqE')
streams = yt.streams

video_best = streams.order_by('resolution').desc().first()
video_480 = streams.filter(res='480p').desc().first()
audio_best = streams.filter(only_audio=True).desc().first()

video_best.download()
video_480_res.download()
audio_best.download()
```

#### `pytube3 `
https://towardsdatascience.com/build-a-youtube-downloader-with-python-8ef2e6915d97
#### `pafy `
https://www.geeksforgeeks.org/youtube-mediaaudio-download-using-python-pafy/

#### `youtube_dl `
https://dev.to/kalebu/how-to-download-youtube-video-as-audio-using-python-33g9 (pip install youtube-dl)
```python
from youtube_dl import YoutubeDL

audio_downloader = YoutubeDL({'format':'bestaudio'})
while True:
    try:
        print('Youtube Downloader'.center(40, '_'))
      	URL = input('Enter youtube url :  ')
      	audio_downloader.extract_info(URL)
    except Exception:
        print("Couldn\'t download the audio")
    finally:
        option = int(input('\n1.download again \n2.Exit\n\nOption here :'))
	    if option!=1:
			break
```
```python
from __future__ import unicode_literals
import youtube_dl

ydl_opts = {}
with youtube_dl.YoutubeDL(ydl_opts) as ydl:
    ydl.download(['http://www.youtube.com/watch?v=BaW_jenozKc'])

#	ou can then set `ydl_opts = {'postprocessors': [{'key': 'FFmpegExtractAudio','preferredcodec': 'mp3','preferredquality': '192'}]}` to download in mp3 format with the given quality.
```
```python
import youtube_dl
def run():
    video_url = input("please enter youtube video url:")
    video_info = youtube_dl.YoutubeDL().extract_info(url = video_url,download=False)
    filename = f"{video_info['title']}.mp3"
    options={
        'format':'bestaudio/best',
        'keepvideo':False,
        'outtmpl':filename,
    }

    with youtube_dl.YoutubeDL(options) as ydl:
        ydl.download([video_info['webpage_url']])

    print("Download complete... {}".format(filename))

if __name__=='__main__':
    run()
```
[`script`: скачать файлы](https://github.com/pythontoday/download_files_python)
_____________
#### Links
[[]]