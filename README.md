# BookMyShow-design



## Table Schema

User table
user_id, user email, password, phone number, address, city, pincode

Movies table
Movie_id, movie_name, ratings, release_date, director, duration

Theather table
theather_id, location, theather_name

Screens table
screen_id, theather_id, screen_type

Shows table
show_id, show_time,, movie_id, show_date

Seats table
seat_id, screen_id, row_number, seat_number

Bookings table
booking_id, show_id, user_id, num_tickets, total_price, booking_time




## Schema Associations

User - bookings,  (One to many)
Movies - Shows,  

Theather - Screens, (one to many)

Screens - Seats,  (one to many)

Shows - Bookings. (One to many)

Seats - 0  (one to one)

Bookings - Show, User (One to Many)






## P1: SQL Queries for table creation and insertion of values:  (P1 task)



Creating and inserting into Movies

CREATE TABLE Movies (
  movie_id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(255) NOT NULL,
  language VARCHAR(50),
  genre VARCHAR(50),
  duration_min INT
);

INSERT INTO Movies (movie_name, ratings, release_date, director, duration)
VALUES ('The Batman (2022)', 8.6, '2022-03-04', 'Matt Reeves', 176),
       ('Lightyear (2022)', 7.3, '2022-06-17', 'Angus MacLane', 100),
       ('Top Gun: Maverick (2022)', 8.6, '2022-05-27', 'Joseph Kosinski', 130);


Creating and inserting into Theatres

CREATE TABLE Theatres (
  theatre_id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(255) NOT NULL,
  location VARCHAR(255) NOT NULL
);

INSERT INTO Theaters (location, theater_name)
VALUES ('Juhu, Mumbai', 'PVR: Andheri'),
       ('Wakad, Pune', 'INOX: Wakad');


Creating and inserting into Screens

CREATE TABLE Screens (
  screen_id INT PRIMARY KEY AUTO_INCREMENT,
  theatre_id INT NOT NULL,
  name VARCHAR(50) NOT NULL,
  FOREIGN KEY (theatre_id) REFERENCES Theatres(theatre_id)
);


INSERT INTO Screens (theater_id, screen_type)
VALUES (1, 'Screen 1 - IMAX'),
       (1, 'Screen 2'),
       (2, 'Screen 1'),
       (2, 'Screen 2 - Dolby Atmos');


Creating and inserting into Shows

CREATE TABLE Shows (
  show_id INT PRIMARY KEY AUTO_INCREMENT,
  movie_id INT NOT NULL,
  screen_id INT NOT NULL,
  show_date DATE NOT NULL,
  show_time TIME NOT NULL,
  FOREIGN KEY (movie_id) REFERENCES Movies(movie_id),
  FOREIGN KEY (screen_id) REFERENCES Screens(screen_id)
);

INSERT INTO Shows (movie_id, show_date, show_time)
VALUES (1, '2024-06-17', '10:00:00'),  -- The Batman (Screen 1 - IMAX)
       (1, '2024-06-17', '15:00:00'),  -- The Batman (Screen 1 - IMAX)
       (2, '2024-06-17', '12:00:00'),  -- Lightyear (Screen 2)
       (3, '2024-06-17', '18:00:00');  -- Top Gun: Maverick (Screen 1)



Creating and inserting into Seats

CREATE TABLE Seats (
  seat_id INT PRIMARY KEY AUTO_INCREMENT,
  screen_id INT NOT NULL,
  row_number INT NOT NULL,
  seat_number INT NOT NULL,
  FOREIGN KEY (screen_id) REFERENCES Screens(screen_id)
);


INSERT INTO Seats (screen_id, row_number, seat_number)
VALUES (1, 1, 1), (1, 1, 2), (1, 1, 3), ..., (1, 10, 10),  -- Seats in Row 1
       (2, 1, 1), (2, 1, 2), (2, 1, 3), ..., (2, 10, 10);  -- Seats in Row 2 (Assuming Screen 2 has similar rows)


Creating and inserting into Bookings

CREATE TABLE Bookings (
  booking_id INT PRIMARY KEY AUTO_INCREMENT,
  show_id INT NOT NULL,
  user_id INT,  -- Optional foreign key if users are implemented
  num_tickets INT NOT NULL,
  total_price DECIMAL(10,2) NOT NULL,
  booking_time DATETIME NOT NULL,
  FOREIGN KEY (show_id) REFERENCES Shows(show_id),
  FOREIGN KEY (user_id) REFERENCES Users(user_id)  -- If users are implemented
);


INSERT INTO Bookings (show_id, user_id, num_tickets, total_price)
VALUES (1, 1, 2, 300.00),  -- John Doe books 2 tickets for The Batman (10:00 AM)
       (3, 2, 1, 150.00);   -- Jane Smith books 1 ticket for Top Gun: Maverick






## P2: Write a query to list down all the shows on a given date at a given theater along with their respective show timings.  



SELECT s.show_time, m.title AS movie_title, t.name AS theatre_name
FROM Shows s
INNER JOIN Movies m ON s.movie_id = m.movie_id
INNER JOIN Screens sc ON s.screen_id = sc.screen_id
INNER JOIN Theatres t ON sc.theatre_id = t.theatre_id
WHERE s.show_date = CURRENT_DATE
  AND t.name = 'PVR: Andheri'; 





