## Before you start (requirements)

You will need to install or have installed a few softwares before you start. Those are just enough to use the programming language. The only extra software is a dependency manager (so it's easy to install the other dependencies).

We won't go in detail on how to install it but a quick search "How to install *name of the software* on *name of your operational system*" will probably lead you to plenty tutorials.

You will also need drivers to make your computer able to communicate with Kano devices. Luckly all the required drivers will be in place by just downloading and installing [Kano Code](https://kano.me/app). If you are running a Linux distribution, then you are probably safe already (get in touch if you have issues).

### Javascript/Node.js

- [Node.js v8.10.0 or higher](https://nodejs.org/en/download/)
- [Yarn v1.9.4 or higher](https://yarnpkg.com/en/docs/install)

You can check Node.js and Yarn versions by typing `node --version` and `yarn --version` on your terminal/console and it will print something like `v8.10.0` or `1.9.4`.

### Python 3

We recommend installing [Mu Editor](https://codewith.mu/) as it will install all the requirements for you. In case you want to use other code editor, you will need to install the following software:

- [Python v3.6.5 or higher](https://www.python.org/downloads/release/python-370/)
- [Pip 9.0.1 or higher](https://pip.pypa.io/en/stable/installing/) (already installed if you install python from the link above)

You can check Python and Pip versions by typing `node --version` and `yarn --version` on your terminal/console and it will print something like `Python 3.6.5` or `pip 9.0.1 from /usr/lib/python3/dist-packages (python 3.6)
`.

If your computer had python 2 already installed you might have to add `3` after all your commands, like `python3 --version` and `pip3 --version`. We strongly recommend using [Mu Editor](https://codewith.mu/) if you don't want to deal with this kind of issue.

## Downloading SDK and installing dependencies

### Javascript/Node.js entire project

1. Download the latest Node.js release [here](https://github.com/KanoComputing/community-sdk/releases)
1. Unzip the folder on a folder easy to access by the terminal (for example your home folder)
1. Navigate to your unzipped folder using the terminal. Check how to do it on [Windows](https://www.watchingthenet.com/how-to-navigate-through-folders-when-using-windows-command-prompt.html), [MacOS](https://www.macworld.com/article/2042378/master-the-command-line-navigating-files-and-folders.html) or [Linux](https://www.digitalocean.com/community/tutorials/basic-linux-navigation-and-file-management)
1. Install all the dependencies by running `yarn install`. That might take a while.
1. Connect your Kano device to your computer and make sure they are on if needed (mostly Pixel Kit)
1. Run an example to test if everything is ok:
    - For Pixel Kit: `node examples/pk_stream_frame.js`
    - For Motion Sensor Kit: `node examples/msk_all.js`

### Javascript/Node.js as a module (not ready yet)

1. Create a folder for your project
1. Initialize your project: `yarn init`
1. Add the Community SDK as dependency: `yarn add @Kano/community-sdk`
1. Follow the [documentation](https://github.com/KanoComputing/community-sdk/wiki/Node.js-SDK:-API-Documentation) on to use it in your project.

### Python 3 with Mu Editor

1. Download the latest Node.js release [here](https://github.com/KanoComputing/community-sdk/releases)
1. Unzip the folder on a folder you can find easily
1. Connect your Kano device to your computer and make sure they are on if needed (mostly Pixel Kit)
1. Open Mu Editor and switch to Python 3 mode
1. Run an example to test if everything is ok by loading one of the example files from the unzipped folder:
    - For Pixel Kit: `example_pixel_kit_stream_frame.py`
    - For Motion Sensor Kit: `example_motion_sensor.py`
1. Press the "Run" button on the toolbar

### Python 3 without Mu Editor

1. Download the latest Node.js release [here](https://github.com/KanoComputing/community-sdk/releases)
1. Unzip the folder on a folder easy to access by the terminal (for example your home folder)
1. Navigate to your unzipped folder using the terminal. Check how to do it on [Windows](https://www.watchingthenet.com/how-to-navigate-through-folders-when-using-windows-command-prompt.html), [MacOS](https://www.macworld.com/article/2042378/master-the-command-line-navigating-files-and-folders.html) or [Linux](https://www.digitalocean.com/community/tutorials/basic-linux-navigation-and-file-management)
1. Install all the dependencies by running `pip install -r requirements.txt`
1. Run an example to test if everything is ok:
    - For Pixel Kit: `python example_pixel_kit_stream_frame.py`
    - For Motion Sensor Kit: `python example_motion_sensor.py`

Make sure to use `python3` and `pip3` if needed.
