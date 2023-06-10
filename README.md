# GDown

Download file (works on colab)
```python
!pip install --upgrade --no-cache-dir gdown
url = "gdrive file link"
gdown.download(url=url, output="filename", quiet=False, fuzzy=True)
```

# Audio

Cut all mp3 files in a folder
```Python
!pip install pydub
from pydub import AudioSegment

mypath = "/content/full_audio_files/"
all_music_filenames = [f for f in listdir(mypath) if isfile(join(mypath, f))]

#output folder
!mkdir cut_music
  
for m in all_music_filenames:
    song = AudioSegment.from_mp3(mypath+m)
    first_20_seconds = song[:20000]
    first_20_seconds.export("/content/cut_music/"+m, format="mp3")
```
