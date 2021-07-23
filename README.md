# documentation_brubotics
Documentation developed by the summer 2021 BruBotics interns.

## How to read our documentation?

First, clone this repository in the folder of your choice:
```
git clone https://github.com/mrs-brubotics/documentation_brubotics.git
```
Then, you will need to install Sphinx.
```
sudo apt update
sudo apt install python3-sphinx
```
Because we use ReadTheDocs, you will need to install the sphinx extension (we recommand you to use the pip3 installer):
```
pip3 install sphinx-rtd-theme
```
Then, run this command from the `documentation_brubotics` folder:
```
make html
```
Finally, open `build/html/index.html` in your web browser.
