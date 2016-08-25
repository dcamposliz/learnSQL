#sqlZOO

Exercises from [sqlZOO](http://sqlzoo.net/).

--

SELECT basics

--

1)
	
	SELECT population FROM world
	WHERE name = 'Germany';

--

2)
	
	SELECT name FROM world
	WHERE name IN ('Ireland', 'Iceland', 'Denmark');

--

3)

	SELECT name, area
	FROM world
	WHERE area BETWEEN 200000 AND 250000;

--

SELECT from WORLD Tutorial

--

1)

	SELECT name, continent, population FROM world;

--

2)

	SELECT name FROM world
	WHERE population >= 200000000;

--

3)

	SELECT name, gdp/population
	FROM world
	WHERE population >= 200000000;

--

4)

	SELECT name, population/1000000
	FROM world
	WHERE continent = 'South America';

--

5)

	SELECT name, population
	FROM world
	WHERE name IN ('France', 'Germany', 'Italy');

--

6)

	SELECT name
	FROM world
	WHERE name LIKE 'United%' OR name LIKE '%United';

--

7)

	SELECT name, population, area
	FROM world
	WHERE area > 3000000 OR population > 250000000;

--

8)

Exclusive OR (XOR). Show the countries that are big by area or big by population but not both. Show name, population and area.
Australia has a big area but a small population, it should be included.
Indonesia has a big population but a small area, it should be included.
China has a big population and big area, it should be excluded.
United Kingdom has a small population and a small area, it should be excluded.

	SELECT name, population, area
	FROM world
	WHERE area > 3000000 XOR population > 250000000;

--

9)

	SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2)
	FROM world
	WHERE continent = 'South America';

--

10)

Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000. Show per-capita GDP for the trillion dollar countries to the nearest $1000.

	SELECT name, ROUND(gdp/population, -3)
	FROM world
	WHERE gdp >= 1000000000000;

--

11)

The CASE statement shown is used to substitute North America for Caribbean in the third column. Show the name - but substitute Australasia for Oceania - for countries beginning with N.

	SELECT name,
	CASE WHEN continent = 'Oceania'
		THEN 'Australasia'
		ELSE continent
		END
	FROM world
	WHERE name LIKE 'N%';

--

12)

Show the name and the continent - but substitute Eurasia for Europe and Asia; substitute America - for each country in North America or South America or Caribbean. Show countries beginning with A or B.

	SELECT name,
	CASE
		WHEN continent = 'Europe'
			THEN 'Eurasia'
		WHEN continent = 'Asia'
			THEN 'Eurasia'
		WHEN continent = 'North America'
			THEN 'America'
		WHEN continent = 'South America'
			THEN 'America'
		WHEN continent = 'Caribbean'
			THEN 'America'
		ELSE continent
	END
	FROM world
	WHERE name LIKE 'A%' OR name LIKE 'B%';

--

13)

Put the continents right.

 - Oceania becomes Australasia

 - Countries in Eurasia and Turkey go to Europe/Asia

 - Caribbean islands starting with 'B' go to North America, other Caribbean islands go to South America

 - Order by country name in ascending order

Show the name, the original continent and the new continent of all countries:

	SELECT name, continent,
	CASE
		WHEN continent = 'Oceania'
			THEN 'Australasia'
		WHEN continent = 'Eurasia'
			THEN 'Europe/Asia'
		WHEN continent = 'Caribbean' AND name LIKE 'B%'
			THEN 'North America'
		WHEN continent = 'Caribbean'
			THEN 'South America'
		ELSE continent
	END
	FROM world
	ORDER BY name;

--

SELECT from Nobel Tutorial

1)

	SELECT yr, subject, winner
	FROM nobel
	WHERE yr = 1950;

--

2)

	SELECT winner
	FROM nobel
	WHERE yr = 1962
	AND subject = 'Literature';

--

3)

	SELECT yr, subject
	FROM nobel
	WHERE winner = 'Albert Einstein';

--

4)

	SELECT winner 
	FROM nobel
	WHERE subject = 'Peace'
	AND yr >= 2000;

--

5)

	SELECT *
	FROM nobel
	WHERE subject = 'Literature'
	AND yr >= 1980
	AND yr <= 1989;

--

6)

	SELECT * FROM nobel
	WHERE winner IN ('Theodore Roosevelt',
	                 'Woodrow Wilson',
	                 'Jimmy Carter');
--

7)
	
	SELECT winner
	FROM nobel
	WHERE winner LIKE "John %";

--

8)

	SELECT *
	FROM nobel
	WHERE 
		(yr = 1980  AND subject = 'Physics')
		OR 
		(yr = 1984 AND subject = 'Chemistry');
