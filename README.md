batch-iv-analysis
=================

This project is a GUI program written in python, intended to make it easy for solar cell researchers to characterize the electrical properties of the solar cells they make. The code here takes IV (current-voltage sweep) data files (of various formats, new format additions welcome!) and uses it to find values for the components of the following solar cell circuit model:

![Circuit](https://upload.wikimedia.org/wikipedia/commons/c/c4/Solar_cell_equivalent_circuit.svg)

where `R_S` is the series resistance  
`R_SH` is the shunt resistance,  
`I_D` is the diode current,  
`I_L` is the light current,  
`I` is the current out of the positive terminal of the cell (given)  
and `V` is the voltage across the cell (given)  
which is described by

![Equation](http://upload.wikimedia.org/math/4/7/d/47d17d3c2fe8840d0b3181860bd22f0a.png)

`I_L`, `I_0`, `n`, `R_S` and `R_SH` are found by [nonlinear regression](https://en.wikipedia.org/wiki/Nonlinear_regression) via [`scipy.optimize.curve_fit()`](http://docs.scipy.org/doc/scipy-0.17.0/reference/generated/scipy.optimize.curve_fit.html) and the user can place arbitrary constraints on these fit parameters (such as specifying ranges for them or setting them to a specific value).

### If you find this code useful...
---
Please cite our work:  
[DOI: 10.3390/photonics2041101](http://www.mdpi.com/2304-6732/2/4/1101/htm)  
and feel free to shoot me an email with comments or suggestions. grey [AT] christoforo [DOT] net  
I'd be super happy to accept pull requests or ideas you have for improving this and I can probably help add support for your own custom IV data file input format. 

### How to use
---
- Start the program with `python batch-iv-analysis.py` if you have python and all of the required packages installed
 - If you're running windows and you don't have python installed, you can try running batch-iv-analysis.exe in the exe zip from [the latest release](https://github.com/spraycoater/batch-iv-analysis/releases/latest/) (note that I create a new release whenever I feel like it and so using a release package typically means you're not running the latest code)
- Open the files for analysis with File --> Open (can select multiple files here)
 - A working example of the format of an input file is exampleData.csv, you can open that file to check that everything is working properly. All non-data rows in a valid input file must be preceded with #

### Features
---
- Hover over the column headers for a little blurb on what that column contains
- Double click anywhere in the table to bring up a graph to compare the fitted curve with the raw data associated with the line you clicked (this is a way to quickly check if the fit is good or bad)
 - This feature seems to crash under windows if you do this without closing a previously opened graph window
- Hover over the file name in each column to see the LabVIEW generated summary data for that file (for comparison purposes)
- The numbers shown in the table for Pmax, Vmax, Voc, I/Jsc and FF are calculated from a spline fit to the data.
 - Hovering over these values will show those calculated from the fit to the characteristic solar cell equation 
 - Hovering over other numbers in the table will show +/- 95% confidence intervals for that value
- Can read and plot i,v vs time data files generated by i-v-vs-time-taker. Does no analysis on them currently (but could in the future).

### Files here
---
- **batch-iv-analysis.py**
 - Main python script. Run this to use the tool. You can edit the code in this file directly using your favorite editor.
- **batch-iv-analysis.ui**
 - Contains user interface design for main window, edit with Qt Designer (I used version 5.6.0)
- **batch_iv_analysis_UI.py**
 - Do not edit this file directly. Instead generate it from batch-iv-analysis.ui by issuing:  
`pyuic5 -o batch_iv_analysis_UI.py batch-iv-analysis.ui`
- **interpolate.py**
 - Borrowed [from here](http://pywafo.googlecode.com/svn/trunk/pywafo/src/wafo/interpolate.py) this is needed for SmoothingSpline
- **polynomial.py**
 - Borrowed [from here](http://pywafo.googlecode.com/svn/trunk/pywafo/src/wafo/polynomial.py) this is needed for SmoothingSpline
- **setup.py**
 - Use this file to generate standalone release packages (see instructions below)

### Install
---
#### Windows and MacOS
1. Install Anaconda [from here](https://www.continuum.io/downloads)
1. Run the "Anaconda Prompt" program that was installed in step #1 and type the following in:

  ```
conda update --all
conda install -c qttesting pyqt
conda install -c conda-forge gmpy2
git clone https://github.com/greysAcademicCode/batch-iv-analysis.git
cd batch-iv-analysis
python batch-iv-analysis.py
```

#### Arch Linux
```
pacaur --needed -S git python python-mpmath python-gmpy2 python-sympy python-scipy python-pyqt5 python-pyqt5 python-scikit-umfpack python-matplotlib
git clone https://github.com/greysAcademicCode/batch-iv-analysis.git
cd batch-iv-analysis
./batch-iv-analysis.py
```

#### Ubuntu Linux
```
# TODO: install deps
git clone https://github.com/greysAcademicCode/batch-iv-analysis.git
cd batch-iv-analysis
./batch-iv-analysis.py
```
