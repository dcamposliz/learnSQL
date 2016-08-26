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

--

9)

	SELECT *
	FROM nobel
	WHERE yr = 1980 AND subject <> 'Chemistry' AND subject <> 'Medicine';

--

10)

	SELECT *
	FROM nobel
	WHERE (subject = 'Medicine' AND yr < 1910) OR (subject = 'Literature' AND yr >= 2004);

--

11)

	SELECT *
	FROM nobel
	WHERE winner = 'PETER GRÜNBERG';

--

12)

	SELECT *
	FROM nobel
	WHERE winner = 'EUGENE O\'NEILL';

--

13)

	SELECT winner, yr, subject
	FROM nobel
	WHERE winner LIKE 'Sir%'
	ORDER BY yr DESC, winner;

--

14)

	SELECT winner, subject
	FROM nobel
	WHERE yr=1984
	ORDER BY subject IN ('Physics','Chemistry') ASC, subject, winner;

--

Some Nobel QUIZ answers:

--

	SELECT COUNT(DISTINCT yr)
	FROM nobel
	WHERE
		yr NOT IN
			(	
				SELECT DISTINCT yr
				FROM nobel
			W	HERE subject = 'Medicine'
			);

--

	SELECT yr
	FROM nobel
	WHERE yr NOT IN
		(
			SELECT yr
			FROM nobel
			WHERE subject IN ('Chemistry','Physics')
		);

--

	SELECT DISTINCT yr
	FROM nobel
	WHERE subject = 'Medicine' 
		AND
		yr NOT IN
			(
				SELECT yr
				FROM nobel
				WHERE subject = 'Peace' 
			)
		AND
		yr NOT IN
			(
				SELECT yr
				FROM nobel
				WHERE subject = 'Literature'
			);

--

SELECT within SELECT Tutorial

--

1)

	SELECT name
	FROM world
	WHERE population > 
		(
			SELECT population
			FROM world
			WHERE name = 'Russia'
		);

--

2)

	SELECT name
	FROM world
	WHERE continent = 'Europe'
		AND gdp / population > 
			(
				SELECT gdp / population
				FROM world
				WHERE name = 'United Kingdom'
			);

--

3)

	SELECT name, continent
	FROM world
	WHERE continent IN
		(
			SELECT continent
			FROM world
			WHERE name = 'Argentina'
		)
		OR continent IN
		(
			SELECT continent
			FROM world
			WHERE name = 'Australia'
		)
	ORDER BY name;

--

4)

	SELECT name, population
	FROM world
	WHERE population >
		(
			SELECT population
			FROM world
			WHERE name = 'Canada'
		)
		AND population <
		(
			SELECT population
			FROM world
			WHERE name = 'Poland'
		);

--

5)

	SELECT name, 
	CONCAT(ROUND(population / (SELECT population FROM world WHERE name = 'Germany') * 100, 0), '%')
	FROM world
	WHERE continent = 'Europe';

--

6)

	SELECT name
	FROM world
	WHERE gdp > ALL
		(
			SELECT gdp
			FROM world
			WHERE continent = 'Europe'
				AND gdp > 0
		);
	
--

7)

	SELECT continent, name, area
	FROM world x
	WHERE area >= ALL
		(
			SELECT area
			FROM world y
			WHERE y.continent = x.continent
				AND area > 0
		);

THIS IS A HARD ONE

--

8)

	SELECT continent, name
	FROM world x
	WHERE name <= ALL
		(
			SELECT name
			FROM world y
			WHERE y.continent = x.continent
			ORDER BY name
		);
 
--

9)

	SELECT name, continent, population
	FROM world x
	WHERE 25000000 >= ALL
		(
			SELECT population
			FROM world y
			WHERE x.continent = y.continent
			AND population > 0
		);

--

10)

	SELECT name, continent
	FROM world x
	WHERE population > ALL
		(
			SELECT population * 3
			FROM world y
			WHERE x.continent = y.continent
			AND x.name <> y.name
			AND population > 0
		);

--

Some Answers to Nested SELECT Quiz

--

1)

	SELECT name, region, population
	FROM bbc x
	WHERE population <= ALL 
		(
			SELECT population
			FROM bbc y
			WHERE x.region = y.region
			AND population > 0
		);

--

2)

	SELECT name, region, population
	FROM bbc x
	WHERE 50000 < ALL
		(
			SELECT population
			FROM bbc y
			WHERE x.region = y.region
			AND population > 0
		);

--

3)

	SELECT name, region
	FROM bbc x
	WHERE population < ALL
		(
			SELECT population / 3
			FROM world
			WHERE x.region = y.region
			AND x.name <> y.name
			AND population > 0
		);

--

4)

	SELECT name
	FROM bbc
	WHERE population >
		(
			SELECT population
			FROM bbc
			WHERE name = 'United Kingdom'
		)
		AND region IN
		(
			SELECT region
			FROM bbc
			WHERE name = 'United Kingdom'
		);

--

5)

	SELECT name
	FROM bbc
	WHERE gdp > ALL 
		(
			SELECT MAX(gdp)
			FROM bbc
			WHERE region = 'Africa'
			AND gdp IS NOT NULL
		);

--

6)

	SELECT name 
	FROM bbc
	WHERE 
		population < (
			SELECT population
			FROM bbc
			WHERE name = 'Russia'
			)
	AND
		population > (
			SELECT population
			FROM bbc
			WHERE name = 'Denmark'
			);

--

SUM and COUNT

1)

	SELECT SUM(population)
	FROM world

--

2)

	SELECT DISTINCT continent
	FROM world

--

3)

	SELECT SUM(gdp)
	FROM world
	WHERE continent = 'Africa'

--

4)

	SELECT COUNT(name)
	FROM world
	WHERE area >= 1000000;

--

5)

	SELECT SUM(population)
	FROM world
	WHERE name IN ('France','Germany','Spain');

--

6)

	SELECT continent, count(name)
	FROM world
	GROUP BY continent
	HAVING COUNT(name);

--

7)

	SELECT continent, count(name)
	FROM world
	WHERE population > 10000000
	GROUP BY continent
	HAVING COUNT(name);

--

8)

	SELECT continent
	FROM world
	GROUP BY continent
	HAVING SUM(population) > 100000000;

--

SUM and COUNT Quiz

--

1)

	SELECT SUM(population)
	FROM bbc
	WHERE region = 'Europe';

--

2)

	SELECT COUNT(name)
	FROM bbc
	WHERE population < 150000;

--

3)

	AVG()
	COUNT()
	FIRST()
	LAST()
	MAX()
	MIN()
	SUM()

--

4)

	SELECT region, SUM(area)
	FROM bbc
	WHERE SUM(area) > 15000000
	GROUP BY region

INCORRECT STATEMENT

--

5)

	SELECT AVG(population)
	FROM bbc
	WHERE name IN ('Poland', 'Germany', 'Denmark');

--

6)

	SELECT region, SUM(population)/SUM(area) as density
	FROM bbc
	GROUP BY region;

--

7)

	SELECT name, population/area AS density
	FROM bbc
	WHERE population = (
			SELECT MAX(population)
			FROM bbc
		);

--

8)

	SELECT region, SUM(area)
	FROM bbc
	GROUP BY region
	HAVING SUM(area) <= 20000000;

--

The nobel table can be used to practice more SUM and COUNT functions.

--

1)

	SELECT COUNT(winner)
	FROM nobel;

--

2)

	SELECT DISTINCT subject
	FROM nobel;

--

3)

	SELECT COUNT(subject)
	FROM nobel
	WHERE subject = 'Physics';

--

4)

	SELECT subject, COUNT(subject)
	FROM nobel
	GROUP BY subject;

--

5)

	SELECT DISTINCT subject, yr
	FROM nobel x
	WHERE yr <= ALL (
			SELECT yr
			FROM nobel y
			WHERE x.subject = y.subject
			ORDER BY yr
		);

--

6)

	SELECT subject, COUNT(subject)
	FROM nobel
	WHERE yr = 2000
	GROUP BY subject;


--

7)

	SELECT subject, COUNT(DISTINCT winner)
	FROM nobel
	GROUP BY subject;

--

8)

	SELECT subject, COUNT(DISTINCT yr)
	FROM nobel
	GROUP BY subject;

--

9)

	SELECT yr
	FROM nobel
	WHERE subject = 'Physics'
	GROUP BY yr
	HAVING COUNT(subject) = 3;

--

10)

	SELECT DISTINCT winner
	FROM nobel
	GROUP BY winner
	HAVING COUNT(winner) > 1;

--

11)

	SELECT DISTINCT winner
	FROM nobel
	WHERE 
	HAVING COUNT(winner) > 1;


--

The JOIN operation

