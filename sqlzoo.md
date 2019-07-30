Solutions to SQL exercises on [sqlzoo](https://sqlzoo.net/wiki/SQL_Tutorial) with notes.

<!-- TOC -->

- [SELECT basics](#select-basics)
- [SELECT names](#select-names)
- [SELECT from World](#select-from-world)
- [SELECT from Nobel](#select-from-nobel)
- [SELECT within SELECT](#select-within-select)
- [SUM and COUNT](#sum-and-count)
- [JOIN](#join)
- [More JOIN operations](#more-join-operations)
- [Using Null](#using-null)

<!-- /TOC -->

# SELECT basics

`world(name, continent, area, population, gdp)`

1. The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes'. Modify it to show the population of Germany.
    ```sql
    SELECT population FROM world
    WHERE name = 'Germany'
    ```
2. The word IN allows us to check if an item is in a list. The example shows the name and population for the countries 'Brazil', 'Russia', 'India' and 'China'. Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.
    ```sql
    SELECT name, population FROM world 
    WHERE name in ('Sweden', 'Norway', 'Denmark')
    ```
3. Which countries are not too small and not too big? BETWEEN allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km. Modify it to show the country and the area for countries with an area between 200,000 and 250,000.
    ```sql
    SELECT name, area FROM world 
    WHERE area BETWEEN 200000 AND 250000
    ```

# SELECT names

`world(nam, continent)`

1. Find the country that starts with Y.
    ```sql
    SELECT name FROM world
    WHERE name LIKE 'Y%'
    ```
2. Find the country that ends with Y.
    ```sql
    SELECT name FROM world
    WHERE name LIKE '%Y'
    ```
3. Find the countries that contain the letter x.
    ```sql
    SELECT name FROM world
    WHERE name LIKE '%x%'
    ```
4. Find the countries that end with land.
    ```sql
    SELECT name FROM world
    WHERE name LIKE '%land'
    ```
5. Find the countries that start with C and end with ia.
    ```sql
    SELECT name FROM world
    WHERE name LIKE 'C%ia'
    ```
6. Find the country that has oo in the name.
    ```sql
    SELECT name FROM world
    WHERE name LIKE '%oo%'
    ```
7. Find the countries that have three or more a in the name.
    ```sql
    SELECT name FROM world
    WHERE LOWER(name) LIKE '%a%a%a%'
    ```
8. Find the countries that have "t" as the second character.
    ```sql
    SELECT name FROM world
    WHERE name LIKE '_t%'
    ```
    Note: `_` is single letter wildcard.
9. Find the countries that have two "o" characters separated by two others.
    ```sql
    SELECT name FROM world 
    WHERE name LIKE '%o__o%'
    ```
10. Find the countries that have exactly four characters.
    ```sql
    SELECT name FROM world
    WHERE name LIKE '____'
    ```
11. Find the country where the name is the capital city.
    ```sql
    SELECT name FROM world
    WHERE name = capital
    ```
12. Find the country where the capital is the country plus "City".
    ```sql
    SELECT name FROM world
    WHERE capital = CONCAT(name, ' City')
    ```
    Note: Notice the space in front of 'City'.
13. Find the capital and the name where the capital includes the name of the country.
    ```sql
    SELECT capital, name FROM world
    WHERE capital LIKE CONCAT('%', name, '%')
    ```
14. Find the capital and the name where the capital is an extension of name of the country. You should include Mexico City as it is longer than Mexico. You should not include Luxembourg as the capital is the same as the country.
    ```sql
    SELECT capital, name FROM world
    WHERE capital LIKE CONCAT('%', name, '%') AND LENGTH(capital) > LENGTH(name)
    ```
15. For Monaco-Ville the name is Monaco and the extension is -Ville. Show the name and the extension where the capital is an extension of name of the country.
    ```sql
    SELECT name, REPLACE(capital, name, '') FROM world
    WHERE capital LIKE CONCAT('%', name, '%') AND LENGTH(capital) > LENGTH(name)
    ```

# SELECT from World

`world(name, continent, area, population, gdp)`

1. Show the name, continent and population of all countries.
    ```sql
    SELECT name, continent, population FROM world
    ```
2.  Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.
    ```sql
    SELECT name FROM world
    WHERE population >= 200000000
    ```
3. Give the name and the per capita GDP for those countries with a population of at least 200 million.
    ```sql
    SELECT name, gdp/population FROM world
    WHERE population >= 200000000
    ```
4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
    ```sql
    SELECT name, population/1000000 FROM world
    WHERE continent = 'South America'
    ```
5. Show the name and population for France, Germany, Italy.
    ```sql
    SELECT name, population FROM world
    WHERE name IN ('France', 'Germany', 'Italy')
    ```
6. Show the countries which have a name that includes the word 'United'.
    ```sql
    SELECT name FROM world 
    WHERE name LIKE '%United%'
    ```
7. A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million. Show the countries that are big by area or big by population. Show name, population and area.
    ```sql
    SELECT name, population, area FROM world
    WHERE area > 3000000 OR population > 250000000
    ```
8.  Show the countries that are big by area or big by population but not both. Show name, population and area.
    ```sql
    SELECT name, population, area FROM world
    WHERE area > 3000000 XOR population > 250000000
    ```
9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the ROUND function to show the values to two decimal places.
    ```sql
    SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2) FROM world
    WHERE continent = 'South America'
    ```
10. Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000 that is 12 zeros). Round this value to the nearest 1000.
    ```sql
    SELECT name, ROUND(gdp/population,-3) FROM world
    WHERE gdp >= 1000000000000
    ```
11. Show the name and capital where the name and the capital have the same number of characters.
    ```sql
    SELECT name, capital FROM world
    WHERE LENGTH(name) = LENGTH(capital)
    ```
12. Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.
    ```sql
    SELECT name, capital FROM world
    WHERE name <> capital AND LEFT(name,1) = LEFT(capital,1)
    ```
13. Find the country that has all the vowels and no spaces in its name.
    ```sql
    SELECT name FROM world
    WHERE name LIKE '%a%' 
    AND name LIKE '%e%'
    AND name LIKE '%i%'
    AND name LIKE '%o%'
    AND name LIKE '%u%'
    AND name NOT LIKE '% %'
    ```

# SELECT from Nobel

`nobel(yr, subject, winner)`

1. Display Nobel prizes for 1950.
    ```sql
    SELECT yr, subject, winner FROM nobel
    WHERE yr = 1950
    ```
2. Show who won the 1962 prize for Literature.
    ```sql
    SELECT winner FROM nobel
    WHERE yr = 1962 AND subject = 'Literature'
    ```
3. Show the year and subject that won 'Albert Einstein' his prize.
    ```sql
    SELECT yr, subject FROM nobel
    WHERE winner = 'Albert Einstein'
    ```
4. Give the name of the 'Peace' winners since the year 2000, including 2000.
    ```sql
    SELECT winner FROM nobel
    WHERE subject = 'Peace' AND yr >= 2000
    ```
5. Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.
    ```
    SELECT * FROM nobel
    WHERE subject = 'Literature' AND yr BETWEEN 1980 AND 1989
    ```
6. Show all details of the presidential winners:

    * Theodore Roosevelt
    * Woodrow Wilson
    * Jimmy Carter
    * Barack Obama
    ```sql
    SELECT * FROM nobel
    WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter', 'Barack Obama')
    ```
7. Show the winners with first name John.
    ```sql
    SELECT winner FROM nobel
    WHERE winner LIKE 'John %'
    ```
8. Show the year, subject, and name of Physics winners for 1980 together with the Chemistry winners for 1984.
    ```sql
    SELECT * FROM nobel
    WHERE (subject = 'Physics' AND yr = 1980) OR (subject = 'Chemistry' AND yr = 1984)
    ```
9. Show the year, subject, and name of winners for 1980 excluding Chemistry and Medicine.
    ```sql
    SELECT * FROM nobel
    WHERE yr = 1980 AND subject NOT IN ('Chemistry', 'Medicine')
    ```
10. Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004).
    ```sql
    SELECT * FROM nobel
    WHERE subject = 'Medicine' AND yr < 1910 OR subject = 'Literature' AND yr >= 2004
    ```
    Note: `AND` has precedence over 'OR'.
11. Find all details of the prize won by PETER GRÜNBERG.
    ```sql
    SELECT * FROM nobel
    WHERE winner = 'PETER GRÜNBERG'
    ```
12. Find all details of the prize won by EUGENE O'NEILL.
    ```sql
    SELECT * FROM nobel
    WHERE winner = 'EUGENE O''NEILL'
    ```
    Note: Single quotes are escaped by doubling them.
13. List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
    ```sql
    SELECT winner, yr, subject FROM nobel
    WHERE winner LIKE 'Sir%'
    ORDER BY yr DESC, winner
    ```
    Note: `ORDER BY` is defaulted to `ASEC`.
14. Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.
    ```
    SELECT winner, subject FROM nobel
    WHERE yr=1984
    ORDER BY subject IN ('Chemistry','Physics'), subject, winner
    ```

# SELECT within SELECT

`world(name, continent, area, population, gdp)`

1. List each country name where the population is larger than that of 'Russia'.
    ```sql
    SELECT name FROM world
    WHERE population >
    (SELECT population FROM world
    WHERE name = 'Russia')
    ```
2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
    ```sql
    SELECT name FROM world
    WHERE continent = 'Europe' AND gdp/population >
    (SELECT gdp/population FROM world
    WHERE name = 'United Kingdom')
    ```
3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
    ```sql
    SELECT name, continent FROM world
    WHERE continent in
    (SELECT continent FROM world
    WHERE name = 'Argentina' OR name = 'Australia')
    ORDER BY name
    ```
4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.
    ```sql
    SELECT name, population FROM world
    WHERE population >
    (SELECT population FROM world WHERE name = 'Canada')
    AND population <
    (SELECT population FROM world WHERE name = 'Poland')
    ```
    Note: Cannot use `BETWEEN...AND` as it is inclusive.
5. Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany. Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
    ```sql
    SELECT name, CONCAT(ROUND(population/(SELECT population FROM world WHERE name = 'Germany')*100), '%') FROM world
    WHERE continent = 'Europe'
    ```
6. Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
    ```sql
    SELECT name FROM world
    WHERE gdp > ALL(SELECT gdp FROM world WHERE 
    continent = 'Europe' AND gdp IS NOT NULL)
    ```
    Or
    ```sql
    SELECT name FROM world
    WHERE gdp > ALL(SELECT gdp FROM world WHERE 
    continent = 'Europe' AND gdp > 0)
    ```
7. Find the largest country (by area) in each continent, show the continent, the name and the area.
    ```sql
    SELECT continent, name, area FROM world x
    WHERE area >= ALL(SELECT area FROM world y
    WHERE y.continent = x.continent AND area > 0)
    ```
    Note: `x` and `y` are aliases.
8. List each continent and the name of the country that comes first alphabetically.
    ```sql
    SELECT continent, name FROM world x
    WHERE name <= ALL(SELECT name FROM world y WHERE y.continent=x.continent)
    ```
9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
    ```sql
    SELECT name, continent, population FROM world x
    WHERE NOT EXISTS 
    (SELECT * FROM world y
    WHERE x.continent = y.continent AND y.population > 25000000)
    ```
    Or
    ```sql
    SELECT name, continent, population FROM world x
    WHERE continent IN (SELECT continent FROM world y
    WHERE 25000000 >= ALL(SELECT population FROM world z 
    WHERE z.continent = y.continent))
    ```
10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.
    ```sql
    SELECT name, continent FROM world x
    WHERE population/3 > ALL(SELECT population FROM world y 
    WHERE y.continent = x.continent AND x.name <> y.name)
    ```

# SUM and COUNT

`world(name, continent, area, population, gdp)`

1. Show the total population of the world.
    ```sql
    SELECT SUM(population) FROM world
    ```
2. List all the continents - just once each.
    ```sql
    SELECT DISTINCT continent FROM world
    ```
3. Give the total GDP of Africa.
    ```sql
    SELECT SUM(gdp) FROM world
    WHERE continent = 'Africa'
    ```
4. How many countries have an area of at least 1000000?
    ```sql
    SELECT COUNT(name) FROM world
    WHERE area >= 1000000
    ```
5. What is the total population of ('Estonia', 'Latvia', 'Lithuania')?
    ```sql
    SELECT SUM(population) FROM world
    WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
    ```
6. For each continent show the continent and number of countries.
    ```sql
    SELECT continent, COUNT(name) FROM world
    GROUP BY continent
    ```
7. For each continent show the continent and number of countries with populations of at least 10 million.
    ```sql
    SELECT continent, COUNT(name) FROM world
    WHERE population >= 10000000
    GROUP BY continent
    ```
8. List the continents that have a total population of at least 100 million.
    ```sql
    SELECT continent FROM world
    GROUP BY continent
    HAVING SUM(population) >= 100000000
    ```
    Note: `WHERE` cannot be used with aggregate functions, so we use `HAVING` instead.

# JOIN

`game(id, mdate, stadium, team1, team2)`

`goal(matchid, teamid, player, gtime)`

`eteam(id, teamname, coach)`

1. Show the matchid and player name for all goals scored by Germany. To identify German players, check for: `teamid = 'GER'`.
    ```sql
    SELECT matchid, player FROM goal
    WHERE teamid = 'GER'
    ```
2. Show id, stadium, team1, team2 for just game 1012.
    ```sql
    SELECT id, stadium, team1, team2 FROM game
    WHERE id = 1012
    ```
3. Show the player, teamid, stadium and mdate for every German goal.
    ```sql
    SELECT player, teamid, stadium, mdate FROM
    game JOIN goal ON id = matchid
    WHERE teamid = 'GER'
    ```
4. Use the same JOIN as in the previous question. Show the team1, team2 and player for every goal scored by a player called Mario `player LIKE 'Mario%'`.
    ```sql
    SELECT team1, team2, player FROM 
    game JOIN goal ON id = matchid
    WHERE player LIKE 'Mario%'
    ```
5. Show player, teamid, coach, gtime for all goals scored in the first 10 minutes `gtime <= 10`.
    ```sql
    SELECT player, teamid, coach, gtime FROM
    goal JOIN eteam ON teamid = id
    WHERE gtime <= 10
    ```
6. List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
    ```sql
    SELECT mdate, teamname FROM 
    game JOIN eteam ON team1 = eteam.id
    WHERE coach = 'Fernando Santos'
    ```
    Note: `id` is a column name in both `game` and `eteam`, so we must specify `eteam.id` instead of just `id`.
7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'.
    ```sql
    SELECT player FROM 
    goal JOIN game ON matchid = id
    WHERE stadium = 'National Stadium, Warsaw'
    ```
8. show the name of all players who scored a goal against Germany.
    ```sql
    SELECT DISTINCT player FROM
    goal JOIN game ON matchid = id
    WHERE team1 = teamid AND team2 = 'GER' OR team2 = teamid AND team1 = 'GER'
    ```
    Or
    ```sql
    SELECT DISTINCT player FROM
    goal JOIN game ON matchid = id
    WHERE (team1 = 'GER' OR team2 = 'GER') AND teamid != 'GER'
    ```
9. Show teamname and the total number of goals scored.
    ```sql
    SELECT teamname, COUNT(gtime) FROM
    goal JOIN eteam ON teamid = id
    GROUP BY teamname
    ```
    Note: `gtime` is unique for each goal.
10. Show the stadium and the number of goals scored in each stadium.
    ```sql
    SELECT stadium, COUNT(gtime) FROM 
    game JOIN goal ON id = matchid
    GROUP BY stadium
    ```
11. For every match involving 'POL', show the matchid, date and the number of goals scored.
    ```sql
    SELECT matchid, mdate, COUNT(gtime) FROM
    game JOIN goal ON matchid = id
    WHERE team1 = 'POL' OR team2 = 'POL'
    GROUP BY matchid, mdate
    ```
12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'.
    ```sql
    SELECT matchid, mdate, COUNT(gtime) FROM
    game JOIN goal ON matchid = id
    WHERE teamid = 'GER'
    GROUP BY matchid, mdate
    ```
13. List every match with the goals scored by each team in  the format
`mdate, team1, score1, team2, score2`.
    ```sql
    SELECT mdate, 
    team1,
    SUM(CASE WHEN teamid = team1 THEN 1 ELSE 0 END) AS score1, 
    team2,
    SUM(CASE WHEN teamid = team2 THEN 1 ELSE 0 END) AS score2
    FROM game LEFT JOIN goal ON matchid = id
    GROUP BY mdate, matchid, team1, team2
    ```
    Note: The `LEFT JOIN` is necessary since there may be matches where both teams score 0 goals, and this situation would not be reflected in the `goals` table. If the default `INNER JOIN` is used, these matches would not be captured in the resulting table.

# More JOIN operations

`movie(id, title, yr, director, budget, gross)`

`actor(id, name)`

`casting(movieid, actorid, ord)`

1. List the films where the yr is 1962 [Show id, title].
    ```sql
    SELECT id, title FROM movie
    WHERE yr = 1962
    ```
2. Give year of 'Citizen Kane'.
    ```sql
    SELECT yr FROM movie
    WHERE title = 'Citizen Kane'
    ```
3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
    ```sql
    SELECT id, title, yr FROM movie
    WHERE title LIKE '%Star Trek%'
    ORDER BY yr
    ```
4. What id number does the actor 'Glenn Close' have?
    ```sql
    SELECT id FROM actor
    WHERE name = 'Glenn Close'
    ```
5. What is the id of the film 'Casablanca'?
    ```sql
    SELECT id FROM movie
    WHERE title = 'Casablanca'
    ```
6. Obtain the cast list for 'Casablanca'.
    ```sql
    SELECT name FROM 
    actor JOIN casting ON id = actorid
    WHERE movieid = 11768
    ```
7. Obtain the cast list for the film 'Alien'
    ```sql
    SELECT name FROM 
    actor JOIN casting ON id = actorid
    WHERE movieid = (SELECT id FROM movie WHERE title = 'Alien')
    ```
8. List the films in which 'Harrison Ford' has appeared.
    ```sql
    SELECT title FROM
    movie JOIN casting ON id = movieid
    WHERE actorid = (SELECT id FROM actor WHERE name = 'Harrison Ford')
    ```
9. List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]
    ```sql
    SELECT title FROM
    movie JOIN casting ON id = movieid
    WHERE actorid = (SELECT id FROM actor WHERE name = 'Harrison Ford') AND ord != 1
    ```
10. List the films together with the leading star for all 1962 films.
    ```sql
    SELECT title, name FROM
    movie JOIN casting ON movie.id = casting.movieid
    JOIN actor ON actor.id = casting.actorid
    WHERE ord = 1 AND yr = 1962
    ```
11. Which were the busiest years for 'John Travolta', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
    ```sql
    SELECT yr, COUNT(id) FROM
    movie JOIN casting ON id = movieid
    WHERE actorid = (SELECT id FROM actor WHERE name = 'John Travolta')
    GROUP BY yr
    HAVING COUNT(id) > 2
    ```
12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.
    ```sql
    SELECT title, name FROM
    movie JOIN casting ON movie.id = casting.movieid
    JOIN actor ON actor.id = casting.actorid
    WHERE ord = 1 AND movieid IN (SELECT movieid FROM casting WHERE actorid = (SELECT id FROM actor WHERE name = 'Julie Andrews'))
    ```
13. Obtain a list, in alphabetical order, of actors who've had at least 30 starring roles.
    ```sql
    SELECT name FROM
    actor JOIN casting ON id = actorid
    GROUP BY name
    HAVING SUM(actorid = id AND ord = 1) >= 30
    ```
14. List the films released in the year 1978 ordered by the number of actors in the cast (descending order), then by title.
    ```sql
    SELECT title, COUNT(actorid) FROM
    movie JOIN casting ON id = movieid
    WHERE yr = 1978
    GROUP BY title
    ORDER BY COUNT(actorid) DESC, title
    ```
15. List all the people who have worked with 'Art Garfunkel'.
    ```sql
    SELECT name FROM
    casting JOIN actor ON actorid = id
    WHERE movieid IN (SELECT movieid FROM casting WHERE actorid = (SELECT id FROM actor WHERE name = 'Art Garfunkel')) AND name != 'Art Garfunkel'
    ```
    Note: Remember to exclude 'Art Garfunkel' himself.

# Using Null

`teacher(id, dept, name, phone, mobile)`

`dept(id, name)`

1. List the teachers who have NULL for their department.
    ```sql
    SELECT name FROM teacher
    WHERE dept IS NULL
    ```
2. List each teacher and their department, excluding teachers with no department and departments with no teacher.
```sql
SELECT teacher.name, dept.name
FROM teacher INNER JOIN dept
ON (teacher.dept=dept.id)
```

TBD