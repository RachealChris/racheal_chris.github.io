---
layout: post
title: Data Analysis - SQL
image: "/posts/coffee_python.jpg"
tags: [SQL, World Data]
---

--Taking a look at the dataset
select *
from facts;

--Determine the maximum and minimum population and population growth in all countries
select 
	   min(population) as MIN_POPULATION,
	   max(population) as MAX_POPULATION,
	   min (population_growth) as MIN_POPULATION_GROWTH,
	   max (population_growth) as MAX_POPULATION_GROWTH
from facts;

--Identifying the countries with the min and max population and population_growth
select *
from facts
where population == (select min(population)
						from facts);
select *
from facts
where population == (select max(population)
						from facts);
--The code above gives us a result of world which is an outlier value, we can remove outliers from the data	
select *
from facts
where population == (select max(population)
						from facts
						where name <> 'World');					
						
--Finding densely populated countries
--Let's get the average population and average area, and we can determine countries that are greater than the average- those will be the more densely populated.

select name, population, area
from facts
where population > (select avg(population) 
						from facts
						where name <> 'World') 
and
	  area < (select avg(area) 
	            from facts	
				where name <> 'World')
				
order by population desc
limit 10;				
	
--Determining the countries likely to face water scarcity

SELECT name, population, population_growth, area_water
from facts
where area_water > (select avg(area_water)
					from facts
					where name <> 'World') 
and population > (select avg(population) 
					from facts
					where name <> 'World')
order by area_water, population;													 

--The result shows that Mexico will likely suffer water scarcity comparing the population to the area water. 
	
	

						
