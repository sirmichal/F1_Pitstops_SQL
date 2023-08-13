# 🏁 Unveiling Formula 1 Qualifying: A Journey Through Time

Get ready to accelerate into the exhilarating realm of Formula 1 racing! Let's try to find valuable insight on how qualifying times has changes over the years. 

## 🔝 What are top 10 driver with most Pole Positions in history?
```SQL
SELECT TOP 10 
	Forename, 
	surname, 
	count(*) as Number_Of_Poles 
FROM 
	qualifying 
JOIN 
	dbo.drivers on dbo.qualifying.driverID = dbo.drivers.DriverID
WHERE 
	dbo.qualifying.position = 1
GROUP BY 
	forename, surname
ORDER BY 
	Number_Of_Poles DESC
```
Answer: "The Top 10 list begins with Lewis Hamilton, who has achieved 106 pole positions, and concludes with Felipe Massa, who has secured 16 pole positions.

## ⏲️ What is the fastest qualifying time per track?
To run the query, firstly we had to change the text falues to NULL.
```SQL
UPDATE qualifying
SET
	q1 = NULLIF(q1, '\N'),
    q2 = NULLIF(q2, '\N'),
    q3 = NULLIF(q3, '\N');

SELECT 
	name, 
	MIN(Least(q1,q2,q3)) as fastest_qualifying_time
FROM 
	qualifying
JOIN dbo.races on dbo.qualifying.raceId = dbo.races.raceId
GROUP BY 
	dbo.races.name
ORDER BY
	fastest_qualifying_time
```
Answer: In total there are 40 different track, I have paste below 10 top fastest tracks.


## Average position in qualifying of Hamilton and Verstappen
As we have two drivers with a surname "Verstappen", we had to add forename condition.
```SQL
SELECT 
	forename,
	surname,
	AVG(position) as Average_position_in_qualifying
 FROM 
	qualifying 
 JOIN 
	drivers on dbo.qualifying.driverId = dbo.drivers.driverId
 WHERE 
	surname = 'Hamilton' or surname = 'Verstappen' and forename = 'Max'
 GROUP BY 
	forename, surname
```
Answer: Hamilton average position in qualifying is 3 and Verstappen is 5.

## Evolution of fastest qualifying time in Monza.
Monza is probably the most known track in F1 calendar. Let's see how the pole position times hase evolved throughout the years.
To run the query, firstly we had to change the text falues to NULL.
```SQL
UPDATE qualifying
SET
	q1 = NULLIF(q1, '\N'),
    q2 = NULLIF(q2, '\N'),
    q3 = NULLIF(q3, '\N');

SELECT 
	YEAR(date) as Year,
	MIN(LEAST(q1, q2, q3)) AS fastest_qualifying_time
FROM 
	qualifying
JOIN
	races on dbo.qualifying.raceId = dbo.races.raceId
WHERE 
	name = 'Monaco Grand Prix'
GROUP BY 
	dbo.races.date
ORDER BY 
YEAR(date)
```
Answer: 


