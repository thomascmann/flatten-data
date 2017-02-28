# flatten-data
Python script for flattening nested data into a list of dicts, designed for importing into a pandas DataFrame.

The script takes a nested data object and returns a list of dicts. Dicts within the object are flattened, and lists within the object are converted into separate dicts within the returned outer list, copying higher-level elements (see example).

### flatten_data(data, simple=0) 

Valid data types are lists and dicts. If a list, the function assumes that the items in the list are individual data records/record groups. If a dict, the function assumes that the outer keys identify records/record groups, and a key in each dict in the returned list will be \_ID_VAR\_, containing this value. 

If the dict reflects a single record/record group, desired performance may be achieved by setting simple = 1 when calling the function.

The design intent of this function is to import into a pandas DataFrame using pandas.io.json.json_normalize.


## Example

#### Flattening a dict containing three data records, importing into Pandas DataFrame:

```python
from pandas.io.json import json_normalize

demo1 = {
	"100": {
		"name": "Tom",
		"job": "student",
		"field": "psychology",
		"grades": [{
			"semester": "2010F",
			"classes": [{
				"class_name": "Calculus 101",
				"grade": "A+"
			}, {
				"class_name": "Psychology 101",
				"grade": "A+"
			}, {
				"class_name": "Writing Seminar",
				"grade": "A"
			}]
		}, {
			"semester": "2011S",
			"classes": [{
				"class_name": "Screenwriting 101",
				"grade": "A-"
			}, {
				"class_name": "Advanced Psychology",
				"grade": "A+"
			}, {
				"class_name": "Writing Seminar II",
				"grade": "A-"
			}, {
				"class_name": "Sailing",
				"grade": "B+"
			}]
		}]
	},
	"101": {
		"name": "Alex",
		"job": "lawyer",
		"field": "law",
		"grades": [{
			"semester": "2008F",
			"classes": [{
				"class_name": "Swimming 101",
				"grade": "B+"
			}, {
				"class_name": "Food 101",
				"grade": "A-"
			}, {
				"class_name": "Law Seminar",
				"grade": "A"
			}]
		}, {
			"semester": "2009S",
			"classes": [{
				"class_name": "Food 102",
				"grade": "A-"
			}, {
				"class_name": "Advanced Law",
				"grade": "F"
			}, {
				"class_name": "Writing with a Writer",
				"grade": "A-"
			}]
		}]
	},
	"102": {
		"name": "Lilly",
		"job": "chef",
		"field": "culinary",
		"grades": [{
			"semester": "2015F",
			"classes": [{
				"class_name": "Molecular Biology",
				"grade": "A+"
			}, {
				"class_name": "Organic Chemistry",
				"grade": "A+"
			}, {
				"class_name": "Organic Food",
				"grade": "A+"
			}, {
				"class_name": "Russian Dressing",
				"grade": "C"
			}]
		}, {
			"semester": "2016S",
			"classes": [{
				"class_name": "Weird Food 101",
				"grade": "C+"
			}, {
				"class_name": "Food Psychology",
				"grade": "A+"
			}, {
				"class_name": "Acting Seminar I",
				"grade": "A-"
			}, {
				"class_name": "Fencing",
				"grade": "A+"
			}]
		}]
	}
}

json_normalize(flatten_data(demo1))
```
Out:
```
	_ID_VAR_	field       grades_classes_class_name	grades_classes_grade  grades_semester job   	name
0   102	    	culinary    Molecular Biology	      	A+	                  2015F	          chef		Lilly
1   102	    	culinary    Organic Chemistry			A+	                  2015F	          chef		Lilly
2   102	    	culinary    Organic Food	          	A+	                  2015F	          chef		Lilly
3   102	    	culinary    Russian Dressing	      	C	                  2015F	          chef		Lilly
4   102	    	culinary  	Weird Food 101            	C+	                  2016S	          chef		Lilly
5   102	    	culinary  	Food Psychology          	A+	                  2016S	          chef		Lilly
6   102	    	culinary  	Acting Seminar				A-	                  2016S	          chef		Lilly
7   102	    	culinary    Fencing                  	A+	                  2016S	          chef		Lilly
8   100	    	psychology	Calculus 101	            A+	                  2010F	          student	Tom
9   100	    	psychology	Psychology 101	          	A+	                  2010F	          student	Tom
10  100	    	psychology	Writing Seminar	          	A	                  2010F	          student	Tom
11	100	    	psychology	Screenwriting 101       	A-	                  2011S	          student	Tom
12	100	    	psychology	Advanced Psychology	      	A+	                  2011S	          student	Tom
13	100	    	psychology	Writing Seminar II	      	A-	                  2011S	          student	Tom
14	100   		psychology	Sailing	                  	B+	                  2011S	          student	Tom
15	101	    	law	        Swimming 101	            B+	                  2008F	          lawyer	Alex
16	101	    	law	        Food 101	                A-	                  2008F	          lawyer	Alex
17	101	    	law	        Law Seminar	              	A	                  2008F	          lawyer	Alex
18	101	    	law	        Food 102	                A-	                  2009S	          lawyer	Alex
19	101	    	law	        Advanced Law	            F	                  2009S	          lawyer	Alex
20	101	   		law	        Writing with a Writer   	A-	                  2009S	          lawyer	Alex
```

