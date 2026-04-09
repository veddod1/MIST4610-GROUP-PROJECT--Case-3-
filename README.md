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






