Problem Statement:

BookMyShow is a ticketing platform where users can book tickets for a movie show. For a given theatre, users can view the next 7 days of available shows and timings.



P1: 
Schema Design: Entities & Attributes
	1.	Theatre: theatre_id (PK), theatre_name, location
	2.	Screen: screen_id (PK), theatre_id (FK → Theatre), screen_number, screen_resolution
	3.	Movie: movie_id (PK), title, language, duration_minutes
	4.	Show: show_id (PK), screen_id (FK - Screen), movie_id (FK - Movie), show_date, show_time, price

Table structure: DDL
-- 1. Theatre
CREATE TABLE Theatre (
  theatre_id     INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  theatre_name   VARCHAR(100) NOT NULL,
  location       VARCHAR(100) NOT NULL
);

-- 2. Screen
CREATE TABLE Screen (
  screen_id         INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  theatre_id        INT UNSIGNED NOT NULL,
  screen_number     INT NOT NULL,
  screen_resolution VARCHAR(50),
  FOREIGN KEY (theatre_id) REFERENCES Theatre(theatre_id)
);

-- 3. Movie
CREATE TABLE Movie (
  movie_id         INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  title            VARCHAR(100) NOT NULL,
  language         VARCHAR(50)  NOT NULL,
  duration_minutes INT NOT NULL
);

-- 4. Show
CREATE TABLE ShowTable (
  show_id    INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  screen_id  INT UNSIGNED NOT NULL,
  movie_id   INT UNSIGNED NOT NULL,
  show_date  DATE NOT NULL,
  show_time  TIME NOT NULL,
  price      DECIMAL(8,2) NOT NULL,
  FOREIGN KEY (screen_id) REFERENCES Screen(screen_id),
  FOREIGN KEY (movie_id)  REFERENCES Movie(movie_id)
);



DML:
INSERT INTO Theatre (theatre_name, location) VALUES
  ('PVR Cinemas', 'Hyderabad'),
  ('INOX',        'Bengaluru');

INSERT INTO Screen (theatre_id, screen_number, screen_resolution) VALUES
  (1, 1, '4K'),
  (1, 2, 'Full HD'),
  (2, 1, '4K');

INSERT INTO Movie (title, language, duration_minutes) VALUES
  ('Avengers: Endgame', 'English', 181),
  ('Spider-Man: No Way Home', 'English', 148),
  ('RRR', 'Telugu', 187);

INSERT INTO ShowTable (screen_id, movie_id, show_date, show_time, price) VALUES
  (1, 1, '2025-08-23', '14:00:00', 350.00),
  (1, 2, '2025-08-23', '17:00:00', 400.00),
  (2, 3, '2025-08-23', '20:00:00', 450.00);


P2: Select query

SELECT m.title, s.show_date, s.show_time
FROM Show s
JOIN Movie m ON s.movie_id = m.movie_id
JOIN Screen sc ON s.screen_id = sc.screen_id
JOIN Theatre t ON sc.theatre_id = t.theatre_id
WHERE t.theatre_name = 'PVR Cinemas' AND s.show_date = '2025-08-24';