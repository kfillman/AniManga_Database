# database creation
# I thought it would be fun to create a database to log the media I've consumed, inlcuding books, TV, and movies

# import nessecary modules
import sqlite3
import csv

# Create database
con = sqlite3.connect('D:/AniManga.bd')
cur = con.cursor()

# Create intial tables
cur.executescript("""
    CREATE TABLE manga(
        manga_id INTEGER PRIMARY KEY AUTOINCREMENT,
        abrev TEXT,
        title TEXT,
        author TEXT,
        status TEXT,
        n_chapters INTEGER,
        n_volume INTEGER
    );
    CREATE TABLE anime(
        anime_id INTEGER PRIMARY KEY,
        abrev TEXT,
        title TEXT,
        studio TEXT,
        n_seasons INTEGER
    );
    CREATE TABLE genre(
        genre_id INTEGER PRIMARY KEY,
        genre TEXT,
        description TEXT
    );
    CREATE TABLE users(
        user_id INTEGER PRIMARY KEY AUTOINCREMENT,
        first_name TEXT NOT NULL,
        last_name TEXT NOT NULL,
        fav_genre TEXT,
        FOREIGN KEY (fav_genre) REFERENCES genre(genre)
    );
    CREATE TABLE reviews(
        review_id INTEGER PRIMARY KEY AUTOINCREMENT,
        UserID INTEGER NOT NULL,
        content_id INTEGER NOT NULL,
        content_type TEXT CHECK(content_type IN ('manga', 'anime')) NOT NULL,
        Rating INTEGER CHECK(Rating >= 1 AND Rating <= 5),
        ReviewDate DATE DEFAULT CURRENT_DATE,
        FOREIGN KEY (UserID) REFERENCES users(user_id),
        CONSTRAINT unique_review UNIQUE (UserID, content_id, content_type)
    );
    CREATE TABLE user_manga_progress(
        user_id INTEGER PRIMARY KEY,
        manga TEXT NOT NULL,
        last_read_chp INTEGER,
        read_status TEXT CHECK(read_status IN ('to-read','in-progress','completed','dnf')) NOT NULL,
        FOREIGN KEY (user_id) REFERENCES users(user_id),
        FOREIGN KEY (manga) REFERENCES manga(title)
    );
    CREATE TABLE user_anime_progress(
        user_id INTEGER PRIMARY KEY,
        anime TEXT NOT NULL,
        last_ep_watched INTEGER,
        watch_status TEXT CHECK(watch_status IN ('to-watch','in-progress','completed','dnf')) NOT NULL,
        FOREIGN KEY (user_id) REFERENCES users(user_id),
        FOREIGN KEY (anime) REFERENCES anime(title)
    )
""")

# populate database with intial data
with open("Manga_list.txt", 'r') as csv_file:
    csv_reader = csv.reader(csv_file)
    for row in csv_reader:
        cur.execute("""
                    INSERT INTO manga(manga_id, abrev, title, author, status, n_chapters, n_volume)
                    VALUES(?,?,?,?,?,?,?)
                    """, row)
with open("Anime_list.txt", 'r') as csv_file:
    csv_reader = csv.reader(csv_file)
    for row in csv_reader:
        cur.execute("""
                    INSERT INTO anime(anime_id, abrev, title, studio, n_seasons)
                    VALUES(?,?,?,?,?)
                    """, row)
with open("genres.txt", 'r') as csv_file:
    csv_reader = csv.reader(csv_file)
    for row in csv_reader:
        cur.execute("""
                    INSERT INTO genre(genre_id, genre, description)
                    VALUES (?,?,?)
                    """, row)

con.commit()

sql_query = """SELECT name FROM sqlite_master  
  WHERE type='table';"""

# print all tables in database
cur.execute(sql_query)
print(cur.fetchall())

con.close()
