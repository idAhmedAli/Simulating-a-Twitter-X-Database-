# Simulating a Twitter (X) Database using MySQL

## Introduction
---
*This code sets up a comprehensive database for a social media platform similar to Twitter (X). It includes tables for users, profiles, tweets, and follow relationships. Additionally, it provides stored procedures to facilitate account creation and managing follow relationships between users.*

```sql
# Create a new database named 'platform'
create database platform;

# Switch to the 'platform' database
use platform;

# Create a 'users' table with columns for id, name, email, and created_at
create table users (
    id int auto_increment primary key not null, # Unique identifier for each user
    name varchar(225) not null, # User's name
    email varchar(225) not null, # User's email address
    created_at datetime not null default current_timestamp # Timestamp when the user was created
);

# Create a 'profiles' table with columns for id, biography, location, and website
create table profiles (
    id int auto_increment primary key not null, # Unique identifier for each profile
    biography text, # User's biography
    location varchar(225), # User's location
    website varchar(225) # User's website URL
);

# Create a 'tweets' table with columns for content, publisher, time, and likes
create table tweets (
    content text, # Content of the tweet
    publisher int, # User ID of the tweet's publisher
    time datetime default current_timestamp, # Timestamp when the tweet was created
    likes varchar(225) # List of users who liked the tweet
);

# Create a 'follows' table to represent the follower-followed relationships
create table follows (
    follower_id int, # ID of the follower
    followed_id int # ID of the followed user
);

# Modify the 'created_at' column in the 'users' table to store only the date
alter table users
modify created_at date;

# Add a foreign key constraint to the 'profiles' table linking 'id' to 'users(id)'
alter table profiles
add constraint fk_profiles_id foreign key (id) references users (id);

# Add a 'password' column to the 'users' table
alter table users
add password binary(64);

# Update the 'password' column for all users with a default hashed password
update users set password = md5("password");

# Add a foreign key constraint to the 'tweets' table linking 'publisher' to 'users(id)'
alter table tweets
add constraint fk_publisher foreign key (publisher) references users(id);

# Define a stored procedure 'createAccount' to create a user and their profile
delimiter //
create procedure createAccount (
    username varchar(225), # Username for the new account
    email varchar(225), # Email for the new account
    password binary(64), # Password for the new account
    biography text, # Biography for the new profile
    location varchar(225), # Location for the new profile
    website varchar(225) # Website for the new profile
)
begin
    # Insert a new record into the 'users' table
    insert into users()
    values ('', username, email, '', password);

    # Insert a new record into the 'profiles' table
    insert into profiles ()
    values ('', biography, location, website);
end //
delimiter ;

# Define a stored procedure 'user_follow' to create a follow relationship between users
delimiter //
create procedure user_follow (
    follower varchar(225), # Username of the follower
    followed varchar(225) # Username of the followed user
)
begin
    # Declare variables to hold user IDs
    declare follower_id int;
    declare followed_id int;

    # Get the ID of the follower from the 'users' table
    select id into follower_id from users where username = follower;

    # Get the ID of the followed user from the 'users' table
    select id into followed_id from users where username = followed;

    # Insert a new record into the 'follows' table
    insert into follows(follower_id, followed_id)
    values (follower_id, followed_id);
end //
delimiter ;
```

## Key Components
---
### **1-Database Creation:**
- Create a new database named (platform).
- Use the (platform) database.

### **2-Core Tables:**
- Users Table (users):
- - Columns: id (unique user identifier), name (user's name), email (user's email address), and created_at (account creation date).
- Profiles Table (profiles):
- - Columns: id (unique profile identifier), biography (user's biography), location (user's location), and website (user's website URL).
- Tweets Table (tweets):
- - Columns: content (tweet content), publisher (user ID of the tweet's publisher), time (timestamp of the tweet), and likes (users who liked the tweet).
- Follows Table (follows):
- - Columns: follower_id (ID of the follower) and followed_id (ID of the followed user).

### **3-Table Modifications:**
- Modify the created_at column in the users table to store only the date.
- Add a foreign key constraint to the profiles table linking id to users(id).
- Add a password column to the users table and update it with a default hashed password.
- Add a foreign key constraint to the tweets table linking publisher to users(id).

### **4-Stored Procedures:**
- Account Creation Procedure (createAccount):
- - Used to add a new user along with their profile.
- User Follow Procedure (user_follow):
- - Used to add a follow relationship between users.

## Usage
---
- This code can be used to set up a database for a social media platform.
- The stored procedures facilitate account creation and managing follow relationships.
- The code includes basic settings that can be customized to meet specific requirements.

## Example Execution
---
To create a new account and establish a follow relationship, you can use the stored procedures as follows:

```sql
CALL createAccount('username', 'email@example.com', MD5('password'), 'Bio goes here', 'Location', 'http://website.com');
CALL user_follow('follower_username', 'followed_username');
```


