Solutions to SQL exercises on [sqlzoo](https://sqlzoo.net/wiki/SQL_Tutorial) with notes.

<!-- TOC -->

- [SELECT basics](#select-basics)
- [SELECT from WORLD Tutorial](#select-from-world-tutorial)
- [SELECT within SELECT Tutorial](#select-within-select-tutorial)
- [SELECT from Nobel Tutorial](#select-from-nobel-tutorial)

<!-- /TOC -->

# SELECT basics

`world(name, continent, area, population, gdp)`

1. The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes'. Modify it to show the population of Germany.
    ```
    SELECT population FROM world
    WHERE name = 'Germany'
    ```
2. The word IN allows us to check if an item is in a list. The example shows the name and population for the countries 'Brazil', 'Russia', 'India' and 'China'. Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.
    ```
    SELECT name, population FROM world 
    WHERE name in ('Sweden', 'Norway', 'Denmark')
    ```
3. Which countries are not too small and not too big? BETWEEN allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km. Modify it to show the country and the area for countries with an area between 200,000 and 250,000.
    ```
    SELECT name, area FROM world 
    WHERE area BETWEEN 200000 AND 250000
    ```

# SELECT from WORLD Tutorial

`world(name, continent, area, population, gdp)`

1. Show the name, continent and population of all countries.
    ```
    SELECT name, continent, population FROM world
    ```
2.  Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.
    ```
    SELECT name FROM world
    WHERE population >= 200000000
    ```
3. Give the name and the per capita GDP for those countries with a population of at least 200 million.
    ```
    SELECT name, gdp/population FROM world
    WHERE population >= 200000000
    ```
4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
    ```
    SELECT name, population/1000000 FROM world
    WHERE continent = 'South America'
    ```
5. Show the name and population for France, Germany, Italy.
    ```
    SELECT name, population FROM world
    WHERE name IN ('France', 'Germany', 'Italy')
    ```
6. Show the countries which have a name that includes the word 'United'.
    ```
    SELECT name FROM world 
    WHERE name LIKE '%United%'
    ```
7. A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million. Show the countries that are big by area or big by population. Show name, population and area.
    ```
    SELECT name, population, area FROM world
    WHERE area > 3000000 OR population > 250000000
    ```
8.  Show the countries that are big by area or big by population but not both. Show name, population and area.
    ```
    SELECT name, population, area FROM world
    WHERE area > 3000000 XOR population > 250000000
    ```
9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the ROUND function to show the values to two decimal places.
    ```
    SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2) FROM world
    WHERE continent = 'South America'
    ```
10. Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000 that is 12 zeros). Round this value to the nearest 1000.
    ```
    SELECT name, ROUND(gdp/population,-3) FROM world
    WHERE gdp >= 1000000000000
    ```
11. Show the name and capital where the name and the capital have the same number of characters.
    ```
    SELECT name, capital FROM world
    WHERE LENGTH(name) = LENGTH(capital)
    ```
12. Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.
    ```
    SELECT name, capital FROM world
    WHERE name <> capital AND LEFT(name,1) = LEFT(capital,1)
    ```
13. Find the country that has all the vowels and no spaces in its name.
    ```
    SELECT name FROM world
    WHERE name LIKE '%a%' 
    AND name LIKE '%e%'
    AND name LIKE '%i%'
    AND name LIKE '%o%'
    AND name LIKE '%u%'
    AND name NOT LIKE '% %'
    ```

# SELECT within SELECT Tutorial

`world(name, continent, area, population, gdp)`

1. List each country name where the population is larger than that of 'Russia'.
    ```
    SELECT name FROM world
    WHERE population >
    (SELECT population FROM world
    WHERE name = 'Russia')
    ```
2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
    ```
    SELECT name FROM world
    WHERE continent = 'Europe' AND gdp/population >
    (SELECT gdp/population FROM world
    WHERE name = 'United Kingdom')
    ```
3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
    ```
    SELECT name, continent FROM world
    WHERE continent in
    (SELECT continent FROM world
    WHERE name = 'Argentina' OR name = 'Australia')
    ORDER BY name
    ```
4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.
    ```
    SELECT name, population FROM world
    WHERE population >
    (SELECT population FROM world WHERE name = 'Canada')
    AND population <
    (SELECT population FROM world WHERE name = 'Poland')
    ```
    Note: Cannot use `BETWEEN...AND` as it is inclusive.
5. Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany. Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
    ```
    SELECT name, CONCAT(ROUND(population/(SELECT population FROM world WHERE name = 'Germany')*100), '%') FROM world
    WHERE continent = 'Europe'
    ```
6. Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
    ```
    SELECT name FROM world
    WHERE gdp > ALL(SELECT gdp FROM world WHERE 
    continent = 'Europe' AND gdp IS NOT NULL)
    ```
    Or
    ```
    SELECT name FROM world
    WHERE gdp > ALL(SELECT gdp FROM world WHERE 
    continent = 'Europe' AND gdp > 0)
    ```
7. Find the largest country (by area) in each continent, show the continent, the name and the area.
    ```
    SELECT continent, name, area FROM world x
    WHERE area >= ALL(SELECT area FROM world y
    WHERE y.continent = x.continent AND area > 0)
    ```
    Note: `x` and `y` are aliases.
8. List each continent and the name of the country that comes first alphabetically.
    ```
    SELECT continent, name FROM world x
    WHERE name <= ALL(SELECT name FROM world y WHERE y.continent=x.continent)
    ```
9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
    ```
    SELECT name, continent, population FROM world x
    WHERE NOT EXISTS 
    (SELECT * FROM world y
    WHERE x.continent = y.continent AND y.population > 25000000)
    ```
    Or
    ```
    SELECT name, continent, population FROM world x
    WHERE continent IN (SELECT continent FROM world y
    WHERE 25000000 >= ALL(SELECT population FROM world z 
    WHERE z.continent = y.continent))
    ```
10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.
    ```
    SELECT name, continent FROM world x
    WHERE population/3 > ALL(SELECT population FROM world y 
    WHERE y.continent = x.continent AND x.name <> y.name)
    ```

# SELECT from Nobel Tutorial

TBD