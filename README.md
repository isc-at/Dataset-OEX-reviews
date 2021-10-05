# Dataset OEX Reviews
This data set is my personal log of running reviewy in Open Exchange.   
It is just a table with 504 records and no special features.    
Ready to run experiments on varius way of indexing.   

All data result from the analysis of the OEX web pages.   
The first run collects the url for the Details pages skipping duplicates    
imposed by the "featured" header block.   
The next run collects updates and details of the packages as available.   
Some informatino is missing as it gets inserted dynamically over several   
fetch cycles while I have just 1 from IRIS.   
 
A utility to load and update is included. to use these methods you need to    
create a SSL Configuration named "community" in SMP for client access to 
_community.intersystems.com:443_

## Prerequisites
Make sure you have [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [Docker desktop](https://www.docker.com/products/docker-desktop) installed.

## Installation 
Clone/git pull the repo into any local directory
```
git clone https://github.com/rcemper/Dataset-simple-M-N.git
```
Run the IRIS container with your project: 
```
docker-compose up -d --build
```
## How to Test it
Connect to the containers SMP and examine content in namespace USER
applying the decribed examples or use commandline $system.SQL.Shell()

### Example 1 
- distibution of ratings
```
  SELECT count(id) pkg, stars
  FROM dc_data_rcc.OEX
  group by NVL(stars,-1)
  order by 2 desc
----------------------------------------------
pkg stars
 7   6.0
 1   5.5
 66  5.0
 24  4.5
 14  4.0
 13  3.5
 14  3.0
 6   2.5
 9   2.0
 4   1.5
 4   1.0
 5   0.5
337  0.0
----------------------------------------------
```
### Example 2
- find top rated authors
```
SELECT stars, %exact(author) author
FROM dc_data_rcc.OEX
where stars > 5 group by author order by 1 desc
----------------------------------------------
stars	author
6.0	Michael Braam
6.0	Peter Steiwer
6.0	Guillaume Rongier
6.0	Jose Tomas Salvador
6.0	Lorenzo Scalese
5.5	Evgeny Shvarov
----------------------------------------------
```
