# normalization.md

Database Normalization for ALX Airbnb Database
Objective
The objective of this document is to analyze the provided alx-airbnb-database schema against normalization principles, specifically aiming to ensure it adheres to the Third Normal Form (3NF). This process aims to enhance data integrity, reduce redundancy, and improve the overall efficiency and maintainability of the database.

Initial Schema Definition
Below is the database schema that was reviewed for normalization:

Entities and Attributes
User

user_id: Primary Key, UUID, Indexed

first_name: VARCHAR, NOT NULL

last_name: VARCHAR, NOT NULL

email: VARCHAR, UNIQUE, NOT NULL

password_hash: VARCHAR, NOT NULL

phone_number: VARCHAR, NULL

role: ENUM (guest, host, admin), NOT NULL

created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

Property

property_id: Primary Key, UUID, Indexed

host_id: Foreign Key, references User(user_id)

name: VARCHAR, NOT NULL

description: TEXT, NOT NULL

location: VARCHAR, NOT NULL

pricepernight: DECIMAL, NOT NULL

created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

updated_at: TIMESTAMP, ON UPDATE CURRENT_TIMESTAMP

Booking

booking_id: Primary Key, UUID, Indexed

property_id: Foreign Key, references Property(property_id)

user_id: Foreign Key, references User(user_id)

start_date: DATE, NOT NULL

end_date: DATE, NOT NULL

total_price: DECIMAL, NOT NULL

status: ENUM (pending, confirmed, canceled), NOT NULL

created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

Payment

payment_id: Primary Key, UUID, Indexed

booking_id: Foreign Key, references Booking(booking_id)

amount: DECIMAL, NOT NULL

payment_date: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

payment_method: ENUM (credit_card, paypal, stripe), NOT NULL

Review

review_id: Primary Key, UUID, Indexed

property_id: Foreign Key, references Property(property_id)

user_id: Foreign Key, references User(user_id)

rating: INTEGER, CHECK: rating >= 1 AND rating <= 5, NOT NULL

comment: TEXT, NOT NULL

created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

Message

message_id: Primary Key, UUID, Indexed

sender_id: Foreign Key, references User(user_id)

recipient_id: Foreign Key, references User(user_id)

message_body: TEXT, NOT NULL

sent_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

Normalization Analysis and Adherence to 3NF
The provided database schema already demonstrates a strong adherence to normalization principles, particularly up to the Third Normal Form (3NF). No significant violations were identified that required structural changes to the tables or relationships.

1. First Normal Form (1NF)
Definition: A relation is in 1NF if all attribute domains are atomic (indivisible) and there are no repeating groups of columns.

Adherence: The schema adheres to 1NF.

Each column contains a single, atomic value (e.g., first_name is atomic; phone_number is a single string).

There are no repeating groups of attributes (e.g., you don't see amenity1, amenity2, amenity3 columns).

Each table has a clearly defined primary key (user_id, property_id, booking_id, etc.) which uniquely identifies each row.

Even the location attribute in the Property table, while it could be further decomposed (e.g., into city, state, country), is currently stored as a single VARCHAR string, which makes it atomic for the purpose of 1NF.

2. Second Normal Form (2NF)
Definition: A relation is in 2NF if it is in 1NF and all non-key attributes are fully functionally dependent on the primary key. This primarily addresses tables with composite primary keys.

Adherence: The schema adheres to 2NF.

None of the tables in the provided schema utilize composite primary keys. Each table has a single-column primary key (user_id, property_id, booking_id, etc.).

Therefore, by definition, all non-key attributes in each table are fully dependent on their respective single-column primary keys.

3. Third Normal Form (3NF)
Definition: A relation is in 3NF if it is in 2NF and there are no transitive dependencies. A transitive dependency exists when a non-key attribute is dependent on another non-key attribute (which is not the primary key).

Adherence: The schema adheres to 3NF.

No Transitive Dependencies Found: Each table's non-key attributes are directly dependent on the table's primary key, and not on any other non-key attribute.

Example for Property table: name, description, location, pricepernight, created_at, updated_at all directly describe a specific property_id and don't derive from any other non-key attribute within the Property table. The host_id is a foreign key, correctly linking to the User table where host-specific details (like first_name, last_name, email) are stored. This prevents storing redundant user details in the Property table.

Example for Booking table: start_date, end_date, total_price, and status are all direct attributes of a booking_id. property_id and user_id are foreign keys linking to their respective entities, ensuring that details about the property or user are not duplicated within the Booking table itself.

This consistent approach across all entities (Payment, Review, Message) ensures that data is stored efficiently and logically, minimizing redundancy and maintaining integrity.

Conclusion
The database schema for the ALX Airbnb project, as defined, is robust and well-structured, successfully meeting the requirements of the Third Normal Form (3NF).

This level of normalization provides significant benefits:

Reduced Data Redundancy: Information is stored in one place, preventing inconsistencies and saving storage space. For example, user details are only in the User table, and property details only in the Property table.

Improved Data Integrity: By eliminating update, insertion, and deletion anomalies, the consistency and accuracy of the data are maintained.

Enhanced Database Maintainability: Changes to data (e.g., a user's email address) only need to be made in a single location, simplifying updates and reducing the risk of errors.

Better Query Performance: While joins are required to combine data from related tables, well-normalized tables generally lead to more efficient data retrieval and processing due to their smaller size and focused content.

The current design serves as a strong foundation for a reliable and scalable AirBnB application.
