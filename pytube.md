2022-03-24 12:51
[doc_en](https://pytube.io/en/latest/index.html)
# pytube
#### [EX](https://pythobyte.com/python-script-to-download-youtube-videos-dca8c93c/)
```python
# pip install pytube || pip install pytube3
from pytube import YouTube

youtube_video_url = 'https://www.youtube.com/watch?v=DkU9WFj8sYo'

try:
    yt_obj = YouTube(youtube_video_url)
    filters = yt_obj.streams.filter(progressive=True, file_extension='mp4')
	#«Прогрессивный» поток содержит файл, имеющий как аудио, так и видео.
	#«Адаптивный» поток содержит звук или видео.
	#Атрибуты «MIME_TYPE», «RES» и «FPS» могут использоваться для фильтрации потока, который мы хотим скачать.
    # download the highest quality video
	for mp4_filter in filters:
    	print(mp4_filter)
    filters.get_highest_resolution().download()
	# download(output_path='/Users/pankaj/temp', filename='yt_video.mp4')
    print('Video Downloaded Successfully')
except Exception as e:
    print(e)
```
### Загрузка только аудио с URL YouTube Video
Иногда мы хотим только аудио с YouTube Video URL. Мы можем использовать `get_audio_only ()` Функция для этого.
```python
from pytube import YouTube


youtube_video_url = 'https://www.youtube.com/watch?v=DkU9WFj8sYo'
try:
	yt_obj = YouTube(youtube_video_url)
	yt_obj.streams.get_audio_only().download(output_path='/Users/pankaj/temp', filename='audio')
	print('YouTube video audio downloaded successfully')
except Exception as e:
	print(e)
```
### Загрузка нескольких видео YouTube
```python
from pytube import YouTube

list_urls = ['https://www.youtube.com/watch?v=DkU9WFj8sYo',
             'https://www.youtube.com/watch?v=D5NK5qMM14g']

for url in list_urls:
    try:
        yt_obj = YouTube(url)
        yt_obj.streams.get_highest_resolution().download()
    except Exception as e:
        print(e)
        raise Exception('Some exception occurred.')
    print('All YouTube videos downloaded successfully.')
```
### Загрузите все видео с плейлиста YouTube
```python
from pytube import Playlist

try:
    playlist = Playlist('https://www.youtube.com/playlist?list=PLcow8_btriE11hzMbT3-B1sBg4YIc-9g_')
    playlist.download_all(download_path='/Users/pankaj/temp')
except Exception as e:
    print(e)
```
### Получение информации на Youtube видео метаданные
```python
from pytube import YouTube

try:
    yt_obj = YouTube('https://www.youtube.com/watch?v=DkU9WFj8sYo')
    print(f'Video Title is {yt_obj.title}')
    print(f'Video Length is {yt_obj.length} seconds')
    print(f'Video Description is {yt_obj.description}')
    print(f'Video Rating is {yt_obj.rating}')
    print(f'Video Views Count is {yt_obj.views}')
    print(f'Video Author is {yt_obj.author}')

except Exception as e:
    print(e)
```


_____________
#### Links
[GUI_Tkinter](https://techvidvan.com/tutorials/youtube-video-downloader-in-python/) &  [GUI_Tkinter](https://www.section.io/engineering-education/youtube-video-downloader-using-python/)
https://codefires.com/download-youtube-videos-using-python-pytube/
https://pypi.org/project/pytube/
**https://pytube.io/en/latest/user/quickstart.html#downloading-a-video**
[[]]