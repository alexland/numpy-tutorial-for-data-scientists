NumPy code snippets, tips, and shortcuts useful in machine learning work


suppose you want to analyze a data set using NumPy; the data set is in a csv file, whose first couple of lines look like this:

```a,all,also,an,and,any,are,as,at,be,been,but,by,can,do,down,even,every,for,from,had,has,have,her,his,if,in,into,is,it,its,may,more,must,my,no,not,now,of,on,one,only,or,our,should,so,some,such,than,that,the,their,then,there,things,this,to,up,upon,was,were,what,when,which,who,will,with,would,your,BookID,Author
46,12,0,3,66,9,4,16,13,13,4,8,8,1,0,1,5,0,21,12,16,3,6,62,3,3,30,3,9,14,1,2,6,5,0,10,16,2,54,7,8,1,7,0,4,7,1,3,3,17,67,6,2,5,1,4,47,2,3,40,11,7,5,6,8,4,9,1,0,1,Austen
35,10,0,7,44,4,3,18,16,9,3,9,14,2,5,1,2,4,14,8,14,2,6,42,1,2,22,5,11,21,1,1,7,4,8,10,11,3,57,10,5,2,6,1,4,11,7,6,5,17,76,6,2,7,0,7,48,1,1,27,13,5,7,7,3,5,14,8,0,1,Austen
```

the first line is of course the *field (column) headers*; the rest are the data points, each data point or row comprised of a sequence of comma-delimited fields, the last of which is the response variable, a text string, which in this instance is the author's name.

```python
import numpy as NP
```

### Namespaces

start by importing the core NumPy library; here i've rebound it to "NP" to keep our namespace clean. This is important because many NumPy modules and functions also have python counterparts--e.g., random, log, exp, pi. Likewise, it's routine to use NumPy, SciPy, and Matplotlib together, and each of these libraries has module names and functions within those modules having the same names (e.g., _eig_ is a function in NumPy's _Linalg_ module; SciPy also has a function called _eig_ in a module also called _linalg_; what's more these two identically named functions are not identical).

### Reading in a CSV file

* remove the first line (header) and store it elsewhere

* read in each row, interpret each comma as a field delimiter
	
* coerce the data item in each row/field (aka "cell") to the correct data type, which in this case is a float

* convert the last field to an integer and build a look-up table to convert back and forth

* stack the rows to give a 2D NumPy array

	
#### Using NumPy, you can perform all of these steps with a single line of code:

assume the file has a name and location bound to *fname*

```python 
fname = "/path/to/file/authorship.csv"
```
	
then call NumPy's *loadtxt*

```python
Data = NP.loadtxt(fname, delimiter=',', skiprows=1, converters={-1:fnx})
```
	
*loadtxt* has a more complex method signature, but much of the time, these four parameters are all that you will need.

The first two are self-explanatory. The third *skiprows* just tells *loadtxt* to skip over the header row.
The fourth is also straightforward. Remember that the last field in each row is the response variable, 
a text string. The *converters* parameter lets us convert any column (aka 'field) in our data on the fly. 
The *converters* parameter takes a python dictionary argument, in which the key is the column number,
and the value is some user-defined function to convert each item in that column.

```python
file_obj = open(fname, "r")
data = [ row.strip().split(",") for row in file_obj.readlines() ]
class_labels = [row[-1] for row in data[1:]]
tx = [label for label in enumerate(class_labels)]
LuT = dict([ (v, k) for k, v in tx ])
fnx = lambda k : LuT[k]
    
NP.random.shuffle(data)
data, class_labels = NP.hsplit(data, [-1])
```

### Reading in a JSON file:

```python
import json as JSON

with open("/path/to/some/file.json", 'rb') as fh:
    Data = JSON.load(fh) 

```

	
## Initial Processing

* removing the class labels

* mean centering, std=1 for all features

* (e.g., for MLP) all features are rescaled so range is (-1, 1)


## Some Basic Description Statistics

* class distribution

* distribution of each variable
	
* correlation


## Some Interesting & Useful Things to do With Your Data

* visualization via multi-dimensional scaling
	
* Clustering

* which variables matter?

    * PCA, LDA
 
    * matrix rank


## Various Common Data Manipulations

* how many data rows in each class
	
* what is the mean and standard deviation for each class?
	
* percent of values in col 10 less than 5?
	
* sort the data by the value of a given column, according to each class
	
* random sample 100 data points from the population
	
* representing the data as an image


