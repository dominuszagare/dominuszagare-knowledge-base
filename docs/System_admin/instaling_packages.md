# instaling software packages on linux
Installing from web sources:
`sudo dnf install curl -y`
or:
`sudo dnf install wget -y`

depending on the system we use different package managers:
- `dnf` for fedora
- `apt` for debian
- `pacman` for arch

## adding snap support

On fedora we can install snap support with: `sudo dnf install snapd`
After that we need to enable snap support: `sudo ln -s /var/lib/snapd/snap /snap`

## example instaling anaconda

1. get instaler resource from web [archive](https://repo.anaconda.com/archive/)
2. download instaler `curl --output anaconda.sh https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh`
3. run instaler `bash anaconda.sh`

### launch anaconda

Launching anaconda navigator: `anaconda-navigator`
Choosing environment: `conda activate myenv`
Creating new environment: `conda create --name myenv`
removing environment: `conda remove --name myenv --all`
Installing packages in environment: `conda install -n myenv numpy`
Activating environment: `conda activate myenv`
Exporting environment: `conda env export > environment.yml`
Importing environment: `conda env create -f environment.yml`