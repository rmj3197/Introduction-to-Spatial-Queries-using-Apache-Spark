# Introduction-to-Spatial-Queries-using-Apache-Spark
Data is the new fuel. And with the introduction of Big Data systems, we have been able to extract more and more useful information from large data files. In this project, we work with Geo-Spatial data. Geo-Spatial Data is about objects, events, or phenomena that have a location on the surface of the earth. Geo-Spatial data combines location information (usually coordinates on the earth), attribute information (the characteristics of the object, event, or phenomena concerned), and often also temporal information (the time or life span at which the location and attributes exist). Geo-Spatial databases support a special type of query known as Spatial Query that allows the use of points, lines, and polygons. This project aims at developing and running multiple such spatial queries on a large database containing geographic data and real-time location data of customers obtained from a peer-topeer taxicab firm.


 # DESCRIPTION OF SOLUTION 

## A. Problem Statement 
•	Range Query: Using the ST_Contains function implemented, given a rectangular region R and a set of points P, we find all the points within. 
•	Range join query: Using the ST_Contains function implemented, given a set of Rectangles R and a set of Points P, we find all (Point, Rectangle) pairs such that the point is within the rectangle. 
•	Distance query: Using the ST_Within function implemented, given a point location P and distance D in km, we find all the points that lie within a distance D from P. 
•	Distance join query: Using the ST_Within function, given a set of Points S1 and a set of Points S2 and a distance D in km, we find all the (s1, s2) pairs such that s1 is within a distance D from s2 where s1 belongs to S1 and s2 belongs to S2. 
•	Hot-Zone Analysis: This task will need to perform a range join operation on a rectangle dataset and a point dataset. For each rectangle, the number of points located within the rectangle will be obtained. More points a rectangle includes, the hotter it is. This task is to calculate the “hotness” of all the rectangles. 
•	Hot-Cell Analysis: This task involves applying spatial statistics on spatial-temporal data using the Getis-Ord statistic to identify statistically significant spatial hot spots. 

## B. Implementation of Solution 

### • ST_CONTAINS: 

ST_CONTAINS takes a point and a rectangle as Input Parameters and Returns a Boolean value depending on whether the point is inside/outside the rectangle. Implementation Steps: 
1. We first split the point into 2 parts: point_x and point point_y which denote its x and y coordinate respectively 
2. We then calculate the upper bound and lower bound of x and y coordinates for the rectangle 
3. If point_x(x coordinate of the point) or point_y(y coordinate of the point) lies above the upper bound or below the lower bound of the rectangle we return false, else we return true. 

### • ST_WITHIN: 

ST_WITHIN takes 2 points and a distance as Input Parameters and Returns a Boolean value depending on whether the distance between the given points is less than or more than the given distance. 

Implementation Steps: 
1. We extract the x and y coordinates of the given points 
2. We then calculate the euclidean distance between the points. 
3. If the calculated distance is Step 2 above is less than the given distance we return true else return false 
4. If point_x (x coordinate of the point) or point_y (y coordinate of the point) lies above the upper bound or below the lower bound of the rectangle we return false, else we return true. 

### • Hot Zone analysis 

Hotzone analysis is used to determine the hotness of rectangular areas. This gives the number of points within each rectangle. This can be determined by doing a range join query on the given rectangular datasets and point datasets and finding the number of points within the rectangle which given the hotness of that zone. 
 
Implementation Steps: 
1. The range join query is achieved using the ST_Contains implemented in the phase 1 of our project and is used here to know the points that lie within the query rectangles. 
2. The result of this range join query is the rectangles and the points that lie within the rectangles. This is stored in joinResult. 
3. We group the joinResult based on the rectangles and then obtain the count which basically tells how many points were contained within each rectangle. The result is later sorted based on the rectangle string 
4. We return the resultant dataframe with the rectangles and their corresponding hotness. 

### • Hot Cell analysis 

The next hotspot analysis task is called HotCell Analysis. It is used to identify statistically significant spatial hotspots. HotCell analysis is carried out with the help of Getis-Ord statistics of NYC Taxi trips data sets. The input point data is a monthly NYC taxi trip dataset (2009-2012) provided in a csv file under resources and is parsed to get the coordinates x, y (latitude, longitude) and z (time – in this case day of the month) for each cell. ‘G score’ is calculated for each pick up point x, y and z (cell) and then the final result is stored in non-increasing order of the ‘G Score’. 

Implementation Steps: 
1. The template file ‘HotcellAnalysis.scala’ has the main function ‘runHotCellAnalysis’ which calculates the g-score and provides results with the help of functions defined in the ‘HotcellUtils.scala’ file. 
2. The function ‘getGetisOrdScore’ calculates g-score for each x-y-z coordinate: 
3. Once the g-score is calculated for x, y,z coordinates, ‘order by’ based on g-score is done and sorted in non-increasing order of g-score and the coordinates are stored in a view named “Result”. This view is then returned by the method ‘runHotCellAnalysis’. 

