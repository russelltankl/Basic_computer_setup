# Basic_computer_setup
This repository is a reference for new computers which needs basic setup.




## Python
### Installation and setup
I usually use dual python version on my computer. Click [here](https://www.python.org/downloads/windows/) to find the Python version you want to install. (Choose 64bit python versions)

During installation of both Python versions, make sure to not add them into PATH and install the location to C:\ .

Afterwords, go to System Environment Variables > Environment Variables > User Variables _ Path. Add the following lines into PATH.

```
C:\Python27
C:\Python27\Scripts
C:\Python37
C:\Python37\Scripts
```

After that, change .exe of each Python version to eg: python.exe > python2.exe.

If using Git Bash, follow the steps below to ensure proper invocation of python. Restart Git Bash after following these steps.

```
cd ~
touch .bashrc
alias py2='winpty python2.exe'
alias py3='winpty python3.exe'
```

### Usage

To start Python interpreter

#### Python2
```
py2
```

#### Python3
```
py3
```

To install packages

#### Python2
```
pip install numpy
```
or
```
py2 -m pip install numpy
```
#### Python3
```
pip3 install numpy
```
or
```
pip3 -m install numpy
```

## Git


## Matlab
To setup Matlab Engine API, first you have to install Matlab first.
