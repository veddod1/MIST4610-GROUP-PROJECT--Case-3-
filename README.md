# MIST4610-GROUP-PROJECT--Case-3-The Peach State Film Festival

## Team Name: 
47114 Group B5 

## Team Members:

1. Aneesh Rudaraju (Conceptual Modeler)
2. Rory Drew (SQL Writer)
3. Kevin Duong (Database Designer/Data Wrangler)
4. Ved Doddapaneni (Group Leader)

## Problem Description:

The Peach State Film Festival (PSFF) is an annual week-long event in Atlanta, Georgia. where films are viewed across multiple theaters. As the festival grew larger, the tracking of film submissions, venue scheduling, and volunteer staffing became inefficient. Our project presents a much more refined relational database solution designed to clearly organize the festival's operations.
Our system handles the entire cycle a film at the festival undergoes. Our database streamlines the festival's operations; It manages the entire process, from the moment a film is submitted and categorized, to the logistics of screenings at various venues and the booking of attendees. Furthermore, it addresses the complex staffing needs of the event, ensuring volunteer shifts are filled and not redundant, and that every volunteer has received the required training.
Beyond organizing the festival's main functions and operations, our database includes an Award System Extension to track the festival's prestigious awards that are given. This system utilizes the award entity to define various awards, the nomination entity to link films to specific awards within a given festival year, and the award_result entity to record final outcomes and the judges' notes. This extension allows the festival to transition from a simple screening event to a data-driven competitive ceremony.

## Data Model

The Film entity is the heart of the model; it serves as the central record for every movie in the festival. It keeps track of the film ID, title, total runtime in minutes, the language of the movie, its country of origin, and the official content rating. To include the "Paired Features" requirement, we implemented a recursive relationship within this table. This allows a film to be optionally linked to one other film (the "paired film") while recording the specific date and type of pairing. We chose to use an internal relationship because it maintains data integrity. This ensures a film is only ever paired with another valid film record without needing a separate  table for optional 1-to-1 pairings.

The Category entity is designed to include the requirement that every film must be submitted to exactly one specific category, such as a Short Narrative or Documentary. It tracks the name of the category code, category name, the maximum runtime allowed for films in that group, and the submission fee. We chose a one-to-many relationship between Category and Film because while a film is restricted to a single category, each category will naturally accommodate a multiple different film submissions.

The Director entity stores the identity of each film's director ID, director, specifically their names and phone numbers. To include the requirement that every film must have exactly one primary director and one assistant director, this entity has two separate many-to-one relationships with the Film table. This design allows the festival to track directors who may work on multiple different movies while enforcing that every single film entry identifies both leadership roles.

The Submitter entity manages the administrative contact for the submitter ID, film, tracking their name, phone number, and email address. We chose a one-to-many relationship between the Submitter and the Film table because the project requires every film to have exactly one submitter. This ensures the festival has only one submitter, even if that person or studio submits several different projects.

The Screening entity manages the schedule by recording the screening ID, specific date, and start time for every showing. It has a many-to-one relationships with both the Film and the Screening Room entities. By creating a unique record for every movie shown in a specific room at a specific time, the system prevents the festival from double-booking a theater or overlapping showtimes.

The Reservation entity records the specific details of an audience member’s booking, including the reservation ID, the date and time they reserved the ticket, the number of seats they claimed, and whether they successfully checked in at the door. It has many-to-one relationships with both the Customer and the Screening. This allows organizers to see in real-time how many seats are filled for a specific screening and helping them identify "no-show" patterns. Moreover, this relationship allows the system to sum up the "seats_reserved" attribute for a specific screening and compare it against the room's total capacity. This prevents the business problem of overselling a theater and ensures a safe, comfortable experience for attendees.

The Customer entity holds the audience's information by the customer ID, customer names, phone numbers, and email addresses of everyone attending the festival. It maintains a one-to-many relationship with the Reservation table. This allows the festival to keep a clean history of every ticket they have ever booked. By allowing one customer to hold many reservations, the festival can track an individual’s entire journey across the event.

The Screening_Room entity handles the specific details of the theaters within a venue, including the screening room ID, room name, its maximum seating capacity, and accessibility flags for closed captioning and ADA support. This entity has a many-to-one relationship with the Venue table. We treat the room as being identified by its parent venue, which solves the problem of distinguishing between rooms with the same name (like "Theater A") across different locations.

The Venue entity tracks the overall physical locations hosting the events, recording the venue ID, building name, its street address, and its contact phone number. It maintains a one-to-many relationship with both the Screening Room and Shift tables. We chose this structure because it serves as the foundation for all required logistics; by linking rooms and work shifts to a specific venue, the festival can ensure that resources are managed at the correct physical site.

The Shift entity defines the labor requirements for the festival, including the shift ID, specific role, the date of the shift, and the start and end times of the shift. It has a many-to-one relationship with the Venue table. This relationship ensures every work shift is tied to a specific physical location where the volunteer is expected to report.

The Assignment entity is the junction that ties a specific volunteer to a specific shift and tracks whether they checked in for their duties. It has many-to-one relationships with both the Volunteer and the Shift tables. This design to include the requirement that volunteers cannot have overlapping shifts; by centralizing assignments, the system can ensure that no individual is scheduled for two different roles at the same time.

The Volunteer entity manages the festival's workforce, tracking volunteer IDs, names, contact info, and whether they have completed their required training. It holds a one-to-many relationship with the Assignment table. This allows organizers to verify a volunteer’s training status before they are assigned to any specialized shifts.

The Award entity identifies the various honors given out at the festival. It tracks the name of the award ID, award, the competition category it belongs to, and a description of what the award recognizes. This entity maintains a one-to-many relationship with the Nomination table. By having a master list of awards, the festival can reuse the same award categories every year. It can simply attach a new set of nominees to them for each new festival cycle without cluttering the database and allowing the database to be reused annually.

The Nomination entity acts as the bridge that connects a film to a specific award. It tracks the nomination ID, the specific year of the festival, and the date the film was officially nominated. This entity holds many-to-one relationships with both the Film and the Award entities. This design allows a single standout film to be nominated for several different awards (like Best Director and Best Picture) simultaneously. It also ensures that each individual award has a clear, organized list of competing films for that specific year.

The Award_Result entity holds the final outcome of the competition. It records the award result ID, whether the film was the official winner, and includes the judges' notes about the result. It maintains a one-to-one relationship with the Nomination entity. We chose this structure because it separates the final result from the initial nomination. This allows organizers to look back at previous years and see exactly who was a finalist (Nomination) versus who actually won (Award Result), ensuring the festival's history of excellence is accurately archived.

<img width="558" height="812" alt="Screenshot 2026-04-09 at 12 51 49 AM" src="https://github.com/user-attachments/assets/714b00aa-d0da-4fe8-9b0f-bc1872ace88e" />

## Data Dictionary
<img width="551" height="646" alt="Screenshot 2026-04-09 at 12 53 11 AM" src="https://github.com/user-attachments/assets/c7acd1e3-4440-4db5-b9ff-f7705ea9143a" />
<img width="551" height="685" alt="Screenshot 2026-04-09 at 12 53 32 AM" src="https://github.com/user-attachments/assets/f3423d09-cb2d-4bd4-8c3d-fc2cd4ecbd57" />
<img width="551" height="685" alt="Screenshot 2026-04-09 at 12 53 52 AM" src="https://github.com/user-attachments/assets/0327ba25-53ee-461a-940c-fa2526e15c95" />
<img width="470" height="435" alt="Screenshot 2026-04-09 at 1 10 12 AM" src="https://github.com/user-attachments/assets/19bf4ace-f0fb-485b-840d-a4a677e7c047" />
<img width="517" height="575" alt="Screenshot 2026-04-09 at 1 10 29 AM" src="https://github.com/user-attachments/assets/5b22bc2f-6665-4e97-9b32-f7e7888c80e9" />
<img width="540" height="526" alt="Screenshot 2026-04-09 at 1 10 39 AM" src="https://github.com/user-attachments/assets/d0d124e9-3adc-4da5-983d-db581ca5945b" />
<img width="502" height="645" alt="Screenshot 2026-04-09 at 1 11 05 AM" src="https://github.com/user-attachments/assets/261765fc-c536-4dcb-96fc-82c14ed63a3c" />


## Queries
<img width="741" height="315" alt="Screenshot 2026-04-09 at 12 55 57 AM" src="https://github.com/user-attachments/assets/9f246a87-1734-4ff1-82df-ac78fe08f778" />

Query 1 lists all films that are rated PG-13 or R and have a runtime longer than 90 minutes, showing the title, content rating, and runtime, ordered from longest to shortest runtime. Query 1 is useful for finding results for later in the evening when certain movies usually play that are longer and more adult-themed. Because they aren’t suited for early slots, timing matters for smooth scheduling alongside kid-friendly picks. Doors might need staff checking IDs before entry, depending on the film’s rating. The festival guide includes warnings for parents, pulled straight from the filtered results. Runtime plus classification are checked together, so only specific titles appear. Instead of scanning every single movie one by one, organizers get a narrowed-down group right away. Each result meets several setup conditions just by showing up in this search. Fewer steps mean less effort tracking what each screening demands behind the scenes. 
<img width="609" height="362" alt="Screenshot 2026-04-09 at 12 57 11 AM" src="https://github.com/user-attachments/assets/dbad6cbc-092e-4b40-af7a-eb0dee1ff3f6" />

Query 2 lists each film category alongside its standard submission fee, the number of films submitted to it, and the estimated revenue that category generated, ordered from highest to lowest revenue. Query 2 allows festival administrators to evaluate the financial performance of each submission category. Most money comes from categories where lots of people send films. When certain types get few entries, it might mean changes are needed - either better promotion or different pricing. Festival staff look at these numbers closely before planning next year's spending. How many sign up affects choices about time set aside for screenings and how much effort goes into advertising specific sections. 
<img width="1011" height="293" alt="Screenshot 2026-04-09 at 1 01 37 AM" src="https://github.com/user-attachments/assets/8583a6a3-66dc-4b84-971b-c94583047c0f" />

Query 3 lists all customers who have made reservations during the festival, showing their name, email, phone number, and total number of reservations made, ordered from most to fewest reservations. Query 3 helps the festival team find out what attendees show up the most. People booking more than one screening stand out because they’re clearly into the event. These guests tend to come back year after year, showing real interest. Instead of guessing, the team can offer them first dibs next time, special pricing, or front-row treatment. The results are sorted by the total amounts of reservations so that way the people with the most are shown first. Staff then know exactly who to reach out to, without extra work piling up. 

<img width="659" height="302" alt="Screenshot 2026-04-09 at 1 02 40 AM" src="https://github.com/user-attachments/assets/fd38bfe9-1c9f-4a43-ab82-57622ab8c970" />

Query 4 lists the complete festival screening schedule showing the film title, venue name, room name, screening date, start time, and room capacity, ordered by date, time, and venue name. Query 4 gives operations staff a single unified master schedule covering every screening across all venues for the entire festival week. This view is useful because it helps for coordinating AV equipment setup, volunteer mapping, audience traffic, and specific logistics such as time and capacity. Having capacity visible alongside each screening also allows staff to quickly identify high-demand screenings that may need additional crowd management support. 
<img width="663" height="399" alt="Screenshot 2026-04-09 at 1 03 29 AM" src="https://github.com/user-attachments/assets/afde6d2c-3341-4769-96b0-afbccb6cefe4" />

Query 5 lists all films that have been officially paired together, showing both film titles side by side, the type of pairing, and the date the pairing was established. The query uses a self-join on the film table through the recursive relationship, and the WHERE condition ensures each pair only appears once rather than twice as mirror rows. Query 5 is useful because it allows festival programmers to quickly see all paired screenings in one view. Paired films are often scheduled back to back and marketed together as a single ticketed event, so having both titles visible in the same row simplifies the process of building the festival program. This query gives programmers an instant overview of all pairing commitments without needing to cross-reference individual film records manually which will also help with keeping both the viewers and staff informed. 

<img width="664" height="303" alt="Screenshot 2026-04-09 at 1 04 10 AM" src="https://github.com/user-attachments/assets/cc22ea3e-6bce-4dad-97cd-c9006d3c412d" />

Query 6 lists all films that received at least one nomination but did not win any award, showing the film title, country of origin, content rating, and primary director. It uses a NOT EXISTS subquery to exclude any film that has a linked winning award result. Query 6 allows the programming and communications teams to easily see all of the films who were nominated for an award but unfortunately did not take home the win. These films represent strong candidates for honorable mention features in the post-festival program, along with other media recognition and promotion. Highlighting these films helps the festival demonstrate the amount of quality submissions it received and gives well-deserving films additional exposure they may not otherwise receive. 

<img width="660" height="399" alt="Screenshot 2026-04-09 at 1 04 45 AM" src="https://github.com/user-attachments/assets/8331fe3b-f424-4308-beb7-eee650ca0a08" />

Query 7 lists each nominated film alongside its country of origin, total number of nominations received, total number of wins, and win rate as a percentage, ordered by nominations and wins in descending order. Query 7 allows festival leadership to identify the most popular and best regarded films of the festival at a glance. A high nomination count shows that a film had great reception across multiple award categories, while the win rate percentage reveals which films converted nominations into actual wins. This combined view is perfect for drafting press releases, and writing the festival recap. 
<img width="665" height="292" alt="Screenshot 2026-04-09 at 1 05 16 AM" src="https://github.com/user-attachments/assets/719db034-4f71-4462-8c39-d8879fc0e85b" />

Query 8 lists all volunteers assigned to more than one shift, showing their name, email, total shifts worked, number of shifts they checked in for, and an overall attendance status label of Perfect Attendance, Partial Attendance, or No Check-ins using CASE logic. Query 8 allows the festival staff to evaluate reliability across their most active volunteers. Volunteers with perfect attendance across multiple shifts are strong candidates for recognition programs, leadership roles, or priority invitations to return next year. On the other hand, volunteers flagged as No Check-ins despite multiple assignments represent volunteers who were not as reliable and should not be given crucial roles. Coordinators can follow up with both the best and the ‘worst’ volunteers in terms of reliability to get good feedback from both ends on how the volunteering process can be improved going forward! 

<img width="663" height="310" alt="Screenshot 2026-04-09 at 1 05 57 AM" src="https://github.com/user-attachments/assets/647678ca-e904-414a-ae9e-ae06b1752fe8" />

Query 9 lists each venue alongside its address, total number of screenings held, total seats reserved across all its screenings, and the number of unique customers who made reservations, filtered to only include venues with more than 10 total seats reserved and ordered by total seats reserved. Query 9 allows the festival board to evaluate which venues were the most popular for both the films themselves along with the viewers who reserved seats. Venues with high seat reservation counts and unique customer counts are clearly the most popular and should be prioritized in future venue marketing budget talks. This data also helps identify underperforming venues that may not be worth renewing contracts for or trying to improve for future years to drive better results. 
<img width="911" height="544" alt="Screenshot 2026-04-10 at 1 52 53 PM" src="https://github.com/user-attachments/assets/35074ccd-832b-4709-870f-d7794118cb51" />

Query 10 lists every award winner showing the award name, award category, winning film title, country of origin, primary director name, submitter name, and any result notes, ordered by award category and award name. A subquery in the WHERE clause confirms each winning film appears in the nomination table before being included in results. Query 10 produces the definitive awards winners summary. This is useful to the festival owners and staff because it aids press releases, award certificates, closing night ceremony scripts, and the official festival program. By grouping results by award category and name, this query 
outputs data that can be directly formatted into the ceremony program with minimal work, saving significant administrative time on one the most important deliverables of the festival.
<img width="914" height="479" alt="Screenshot 2026-04-10 at 1 53 17 PM" src="https://github.com/user-attachments/assets/7b3cb60c-9a18-465c-8a18-fcb6642c04e1" />


## Databse Information
Name of the database: mb_B5
Additional information: Each query listed above is marked in the database using stored procedures which can be called using the following format: CALL mbB5_Q1();










