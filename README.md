# documentation_brubotics

Documentation of the droneswarm_brubotics packages.
This repo is automatically cloned when you install droneswarm_brubotics.

## Prerequisites:

* Install pip3:
  ``` bash
  sudo apt install python3-pip
  ```

* Install sphinx:
  ``` bash
  python3 -m pip install sphinx
  ```
  or 

  ``` bash
  sudo apt install python3-sphinx
  ```
* To use ReadTheDocs, install the following sphinx extension:
  ``` bash
  python3 -m pip install sphinx_rtd_theme
  ```

* Install myst-parser:
  ``` bash
  python3 -m pip install myst-parser
  ```

* Install build-essential:
  ``` bash
  sudo apt-get install build-essential
  ```

* Install docutils
  ```bash
  python3 -m pip install docutils==0.16
  ```

## How to read our documentation?

Run this command from the `documentation_brubotics` folder:

```
make html
```

Finally, open `build/html/index.html` in your web browser (NOT the _build folder!).