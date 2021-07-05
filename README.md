# documentation_brubotics
Documentation developed by the summer 2021 BruBotics interns.

## How to visualize our documentation?

First, clone this repository in the folder of your choice:
```
git clone https://github.com/mrs-brubotics/documentation_brubotics.git
```
Then, you will need to install sphinx (we recommand you to use the pip3 installer).
If you are running on Ubuntu 18.04, use:
```
pip3 install sphinx
```
If you are running on Ubuntu 20.04, use:
```
sudo apt-get install python3-sphinx
```
Because we use ReadTheDocs, so you will need to install the sphinx extension:
```
pip3 install sphinx-rtd-theme
```
Then, run this command from the `documentation_brubotics` folder:
```
make html
```
Finally, open `build/html/index.html` in your web browser.
