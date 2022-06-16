CREATE DATABASE bsql_assignment204
use bsql_assignment204

CREATE TABLE Movie(
    MNo INT PRIMARY KEY IDENTITY,
    MName VARCHAR(100) NOT NULL,
    Duration REAL CHECK(Duration >= 1) NOT NULL,
    Genre INT CHECK(Genre >= 1 AND Genre <= 8) NOT NULL,
    Director VARCHAR(100) NOT NULL,
    [Money] MONEY NOT NULL,
    Comments VARCHAR(Max)
)

ALTER TABLE Movie ADD ImageLink VARCHAR(450) UNIQUE

INSERT INTO Movie VALUES
    ('Thua me con di', 1.25, 5, 'Vu Ngoc Dang', 12000000, NULL, 'ecguhcw'),
    ('Chi muoi ba', 2.25, 1, 'Vu Ngoc Dang', 12000000, NULL, 'gfwuh'),
    ('Fast and Furious', 1.25, 1, 'Vu Ngoc Dang', 12000000, NULL, 'yehrij'),
    ('Hotboy noi loan', 3.25, 2, 'Vu Ngoc Dang', 12000000, NULL, 'ehj'),
    ('Love, Simon', 1.25, 3, 'Vu Ngoc Dang', 12000000, NULL, 'ruhgj')

CREATE TABLE Actor(
    ANo INT PRIMARY KEY IDENTITY,
    AName VARCHAR(100) NOT NULL, 
    Age INT NOT NULL,
    Salary MONEY NOT NULL,
    Nationality VARCHAR(100)
)

INSERT INTO Actor VALUES
    ('Huy', 27, 186271894, 'VN'),
    ('Lanh', 30, 623438929, 'VN'),
    ('Huong', 54, 438929, 'VN'),
    ('Van Diesel', 47, 635423438929, 'VN'),
    ('Hai', 35, 13438929, 'VN'),
    ('Simon', 22, 38929, 'VN'),
    ('Tham', 60, 23657234892637, 'VN'),
    ('Luan', 80, 629, 'VN')

UPDATE Actor SET AName = 'Lam' WHERE AName = 'Luan'

CREATE TABLE Actedin(
    MNo INT FOREIGN KEY REFERENCES Movie(MNo),
    ANo INT FOREIGN KEY REFERENCES Actor(ANo),
    PRIMARY KEY(MNo, ANo)
)

INSERT INTO Actedin VALUES
    (1,1),
    (1,2),
    (2,1),
    (2,3),
    (2,5),
    (2,6),
    (2,8),
    (3,4),
    (3,6),
    (4,5),
    (4,8),
    (5,6)

-- Retrieve all the data in the Actor table for actors that are older than 50.
SELECT * FROM Actor
WHERE Age > 50

-- Retrieve all actor names and average salaries from ACTOR and sort the results by average salary.
SELECT AName, Salary FROM Actor
ORDER BY Salary

-- Using an actor name from your table, retrieve the names of all the movies that actor has acted in.
SELECT AName, MName
FROM Actor a
JOIN Actedin ai ON a.ANo = ai.ANo
JOIN Movie m ON m.MNo = ai.MNo
WHERE a.AName = 'Simon'

-- Retrieve the names of all the action movies that amount of actor be greater than 3
SELECT MName FROM Movie
WHERE Genre = 1 AND MNo IN (
    SELECT MNo 
    FROM Actedin 
    GROUP BY MNo
    HAVING COUNT(*) > 3
)
