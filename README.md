# Colab

GDown
Download file (works on colab)
```python
!pip install --upgrade --no-cache-dir gdown
import gdown
url = "gdrive file link"
gdown.download(url=url, output="filename", quiet=False, fuzzy=True)
```

Reload Modules (colab)
```
!pip install Ipython --upgrade
%load_ext autoreload
%autoreload 2
```
# Audio

Cut all mp3 files in a folder
```Python
!pip install pydub
from os import listdir
from os.path import isfile, join
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

Convert all audio files from a folder into mp3, and save to another folder
```python
import os
from pydub import AudioSegment
from pydub.exceptions import CouldntDecodeError

!mkdir mp3_converted

def convert_files_to_mp3(source_folder, destination_folder):
    if not os.path.exists(destination_folder):
        os.makedirs(destination_folder)

    files = os.listdir(source_folder)

    for file_name in files:
        if not file_name.lower().endswith(('.wav', '.mp3', '.ogg', '.flac', '.aac', '.m4a')):
            continue
            
        source_path = os.path.join(source_folder, file_name)
        destination_path = os.path.join(destination_folder, os.path.splitext(file_name)[0] + '.mp3')

        try:
            # Load the audio file
            audio = AudioSegment.from_file(source_path)
        except CouldntDecodeError:
            print(f"Skipping {file_name} - Unable to decode the file.")
            continue

        audio.export(destination_path, format='mp3')

    print("Conversion complete!")

source_folder = '/content/all_music'
destination_folder = '/content/mp3_converted'
convert_files_to_mp3(source_folder, destination_folder)
```
# Conda

Create new env
```bash
conda create --name myenv
```

Activate env
```bash
conda activate myenv
```

Fix wrong python, even when in conda env
```bash
conda create --name env_name python=3.10
```

export conda env info
```
conda env export --no-builds > env.yml
```
to set up: `conda create env_name` `conda activate env_name` `conda env update --file env.yml`

# Zip
Extract contents of a zip into a destination foler
```bash
unzip /content/foo.zip -d /content/destination_folder
```

Zip a folder
```bash
zip -r compressed_filename.zip foldername
```

# File
List all files in a folder

```python
from os import listdir
from os.path import isfile, join
onlyfiles = [f for f in listdir(mypath) if isfile(join(mypath, f))]
```

# Git

copy ssh key from remote machine (assuming already generated), next copy paste this in github GUI as a new key.
```
pbcopy < ~/.ssh/id_rsa.pub
```

Set up username and email on current repo:
```
git config user.name "Mainakdeb"
```

```
git config user.email "mainakmayukh2000@gmail.com"
```

check username or email before pushing
```
git config user.name
```

```
git config user.email
```

which branch am i on?
```
git branch
```

switch to existing branch
```bash
git switch <existing_branch>
```

create new branch and switch to it
```bash
git switch -c <non_existing_branch>
```

in case you commit using the wrong username:
```bash
git rebase -r <some commit before all of your bad commits> \
```
```bash
--exec 'git commit --amend --no-edit --reset-author'
```

git clone from a private repo using personal access token
```bash
git clone https://<PERSONAL_ACCESS_TOKEN>@github.com/username/your_repo.git
```

pull changes from remote repo and rebase
```bash
git pull --rebase origin main <or any branch>
```

rename / move a file
```bash
git mv sourceLocation targetLocation
```
# ssh vscode/cursor

If you get 404 error when trying ssh in cursor (windows)
1. Uninstall your current Remote-SSH extension
2. Install version 0.113 of Remote-SSH from the Cursor marketplace
3. Close any VS Code windows with active SSH connections
4. Restart Cursor completely
5. Try connecting again
6. This should resolve the 404 error 

# Hugo Blog

1. clone
```
git clone https://github.com/Mainakdeb/blog.git
```

2. Create and activate env for hugo dev
```
conda create --name hugo-local-env
conda activate hugo-local-env
```

3. install hugo if needed
```
sudo apt install hugo
```

4. serve locally
```
hugo serve
```

5. Build Site
```
hugo
```

Upload contents from `./blog/public/` into a github repo named as `your_username.github.io`

# SLURM
Allocate resources to yourself from login node (openmind)
```bash
srun -t 12:00:00 --ntasks-per-node=8 --mem=32G --gres=gpu:a100:1 --pty bash
```

Allocate resources (PACE) (gpu v100)
```
salloc -A gts-rmurty7 -q inferno -N1 --ntasks-per-node=1 --gres=gpu:1 -t4:00:00
```

Show running jobs
```bash
watch squeue -u <username>
```

Cancel job by name
```bash
scancel --name <your-job-name>
```

Cancel all running jobs
```bash
scancel -u <username>
```

# SCP (Secure Copy Protocol)
```bash
scp <username>@ssh-host:/path-to-source ./path-to-destination/
```

# Tmux

when in a terminal, and you need another within
```bash
tmux
```

```bash
tmux new-session -s code-tunnel
```

inside tmux, now to get a terminal (dont't forget to activate your conda env after this if needed)
```bash
bash
```

start your stuff, then press "ctrl+b, d" to exit tmux, "ctrl+b, x" will kill

check back whats going on in tmux
```bash
tmux attach -t 0
```

# VSCode port forwarding

in case default port forwarding fails, use ngrok (useful for pycortex or jupyter lab)
```
ngrok http <PORT>
```

when running jupyter lab in vscode inside a slurm cluster (phoenix)

```
jupyter lab --NotebookApp.allow_origin='*' --NotebookApp.ip='0.0.0.0'
```

# Python Package
install a package to edit (from local repo)
```bash
python3 setup.py develop
```

# Mounting Drive (Linux)

find device to mount: 
```bash
df -h
```

mount: 
```
sudo mount /dev/sda /home/penfield/research/mega_storage
```
(You can unmount using `umount`)

check if mounted:
```
sudo df -a -T -h
```

give permissions to mounted device: 
```
sudo chmod -R g+w /home/penfield/research/mega_storage
sudo chmod -R g+w /home/penfield/research/mega_storage
```


# Publishing to pypi
`python setup.py bdist_wheel sdist `
`twine check dist/*`
`twine upload --repository-url https://test.pypi.org/legacy/ dist/*`

if you're using api key, type username __token__ and paste token as password.
