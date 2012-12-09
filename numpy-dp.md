
So we want to analyze a data set using NumPy. The data set is in a csv file, whose first couple of lines look like this:

a,all,also,an,and,any,are,as,at,be,been,but,by,can,do,down,even,every,for,from,had,has,have,her,his,if,in,into,is,it,its,may,more,must,my,no,not,now,of,on,one,only,or,our,should,so,some,such,than,that,the,their,then,there,things,this,to,up,upon,was,were,what,when,which,who,will,with,would,your,BookID,Author
46,12,0,3,66,9,4,16,13,13,4,8,8,1,0,1,5,0,21,12,16,3,6,62,3,3,30,3,9,14,1,2,6,5,0,10,16,2,54,7,8,1,7,0,4,7,1,3,3,17,67,6,2,5,1,4,47,2,3,40,11,7,5,6,8,4,9,1,0,1,Austen
35,10,0,7,44,4,3,18,16,9,3,9,14,2,5,1,2,4,14,8,14,2,6,42,1,2,22,5,11,21,1,1,7,4,8,10,11,3,57,10,5,2,6,1,4,11,7,6,5,17,76,6,2,7,0,7,48,1,1,27,13,5,7,7,3,5,14,8,0,1,Austen

The first line is of course the *field headers*; the rest are the data points, each data point or row comprised of a number of comma-delimited fields, the last of which is the response variable, a text string, which is the author's name.

    import numpy as NP

I. Reading the Data in

start by importing the core NumPy library; here i've rebound it to "NP" to keep our namespace clean. This is important because many NumPy modules and functions also have python counterparts--e.g., random, log, exp, pi. Likewise, it's routine to use NumPy, SciPy, and Matplotlib together, and each of theselibraries has functions with the same name as the other two.

Proper Conversion of this csv file to a NumPy array involves these steps:

	* remove the first line (header) and store it elsewhere

	* read in each row, interpret each comma as a field delimiter
	
	* coerce the data item in each row/field (aka "cell") to the correct data type, which in this case is a float

	* convert the last field to an integer and build a look-up table to convert back and forth

	* stack the rows to give a 2D NumPy array
	
Using NumPy, you can perform all of these steps with a single line of code:

Assume the file has a name and location bound to *fname*

    fname = "/path/to/file/authorship.csv"
	
Then, call NumPy's *loadtxt*.

	Data = NP.loadtxt(fname, delimiter=',', skiprows=1, converters={-1:fnx})
	
*loadtxt* has a more complex method signature, but much of the time, these four parameters are all that you will need.

The first two are self-explanatory. The third *skiprows* just tells *loadtxt* to skip over the header row.
The fourth is also straightforward. Remember that the last field in each row is the response variable, 
a text string. The *converters* parameter lets us convert any column (aka 'field) in our data on the fly. 
The *converters* parameter takes a python dictionary argument, in which the key is the column number,
and the value is some user-defined function to convert each item in that column.

	file_obj = open(fname, "r")
	data = [ row.strip().split(",") for row in file_obj.readlines() ]
	class_labels = [row[-1] for row in data[1:]]
	tx = [label for label in enumerate(class_labels)]
	LuT = dict([ (v, k) for k, v in tx ])
	fnx = lambda k : LuT[k]
    
	NP.random.shuffle(data)
	data, class_labels = NP.hsplit(data, [-1])
	
	
III. Initial Processing:

	* removing the class labels

	* mean centering, std=1 for all features

	* (e.g., for MLP) all features are rescaled so range is (-1, 1)


IV. Some Basic Description Statistics

	* Class Distribution

	* Distribution of each Variable
	
	* correlation


V. Some Interesting & Useful Things to do With Your Data

	* Visualization via Multi-Dimensional Scaling
	
	* Clustering

	* Which Variables Matter?

 		* PCA, LDA
 
 	   * matrix rank


VI. Various Common Data Manipulations

	* how many data rows in each class
	
	* what is the mean and standard deviation for each class?
	
	* Percent of values in col 10 less than 5?
	
	* sort the data by the value of a given column, according to each class
	
	* random sample 100 data points from the population
	
	* representing the data as an image


