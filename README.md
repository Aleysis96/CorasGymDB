# CorasGymDB
# Design Document

By Alexis Cardona

Video overview: <https://youtu.be/ntq7RncfX08?si=h63hYJp-oB1i6qrw>

## Scope

The database for CS50 SQL includes all entities necessary to facilitate the process of tracking a local gym members and memberships
as such, included in the database's scope is:

* Members of the gym
* Type of Memberships
* Record of all memberships with details of memberships types
* Payments made by the members
* Records of attendance of the members
* Employees of the gym

## Functional Requirements

This database will support:

* Register new members
* Record payments made by new or old members of the gym
* Automatically calculate membership expiration dates based on payment and membership duration
* Log attendance using biometric (incorporated by another sofware that support biometrics) or manual check-ins, linked to active memberships.
* Generate reports on member activity, payment status, and attendance trends

## Representation

Entities are captured in SQLite tables with the following schema.

### Entities

The database includes the following entities:

### Members

The `Members` table includes:

`id` which specifies the unique ID for the member as an `INTEGER`. This column thus has the `PRIMARY KEY` constraint applied.
`first_name` which specifies the member's first name as `TEXT`, given `TEXT` is appropriate for name fields.
`last_name` which specifies the member's last name. `TEXT` is used for the same reason as `first_name`.
`date_birth` wich specifies the member's date of birth. `TEXT` is used as the date is writing with special characters and numbers.
(e.g 1995-06-27).
`gender` wich specifies the member's gender. `TEXT` is used with constraints to be used only for 'Female', 'Male', 'Non_binary', 'Prefer_not_to_say'.
`phone_number` wich specifies the member´s phone number. `NUMERIC` is appropiate for phone number fields.

All columns in the `Members` table are required, and hence should have the `NOT NULL` constraint applied.

### Membership

the `Membership` table includes:

`id` which specifies the unique ID for the membership as an `INTEGER`. This column thus has the `PRIMARY KEY` constraint applied.
`membership_type` wich specifies for all different types of memberships within the gym. `TEXT` is used with constraints to be used only for 'General', 'School', 'Daytime'.
`enter_time` wich specifies the entry time according to type of membership. each membership type vary in cost and open times based on the type of membership you are buying. `TEXT` is used with constraints to be used only for '06:00', '10:00'.
`exit_time` wich specifies the exit time according to type of membership. each membership type vary in cost and closing times based on the type of membership you are buying. `TEXT` is used with constraints to be used only for '22:00', '16:00'.
`price` wich specifies the price of the membership. `INTEGER` is used as each membership type does not include decimals in their prices. containt constraints with prices based on the membership type, 1 day: MXN100, 1 week: MXN250, 1 month school type: MXN350, 1 month daytime: MXN500, 1 month general: MXN700, 6 months general: MXN3500, annually general: MXN7500. ONLY 'General' has discount and offers semester and annually memberships.
`membership_duration_days` wich specifies the days of duration for each membership type. `INTEGER` is used as is appropiate for number of days.
`additional_comments` wich specifies any additional comments for the membership. `TEXT` is appropiate for comment fields.

All columns in the `Membership` table are required, and hence should have the `NOT NULL` constraint applied. `additional_comments` may not be necessary sometimes so is leave without contraints.

### Memberships_records

the `Memberships_records` table includes:

`id` which specifies the unique ID for the record as an `INTEGER`. This column thus has the `PRIMARY KEY` constraint applied.
`member_id` which is the ID of the member as an `INTEGER`. This column thus has the `FOREIGN KEY` constraint applied, referencing the `id` column in the `Members` table to ensure data integrity.
`membership_id` which is the ID of the membership type as an `INTEGER`. This column thus has the `FOREIGN KEY` constraint applied, referencing the `id` column in the `Membership` table to ensure data integrity.
`membership_start_date` wich specifies in format of YYYY-MM-DD the start date of the membership. `TEXT` is used as the date is writing with special characters numbers. (e.g 1995-06-27).
`membership_end_date` wich specifies in format of YYYY-MM-DD the end date of the membership. `TEXT` is used as the date is writing with special characters and numbers. (e.g 1995-06-27).
`status` wich specifies the status of the member. `TEXT` is used with constraints to be used only for 'Active', 'Not active'.

All columns in the `Memberships_records` table are required, and hence should have the `NOT NULL` constraint applied.

### Payments

the `Payments` table includes:

`id` which specifies the unique ID for the payment as an `INTEGER`. This column thus has the `PRIMARY KEY` constraint applied.
`membership_record_id` which is the ID of the record as an `INTEGER`. This column thus has the `FOREIGN KEY` constraint applied, referencing the `id` column in the `Memberships_records` table to ensure data integrity.
`payment_date` wich specifies the date of the payment. Timestamps in SQLite can be conveniently stored as `NUMERIC` The default value for the `payment_date` attribute is the current timestamp, as denoted by `DEFAULT CURRENT_TIMESTAMP`.
`amount` wich specefies the amount of the payment. `REAL` is used to stored decimals and integers as payments.
`payment_method` wich specefies the metho used for the payment. `TEXT`is used with constraints to be used only for 'Cash', 'Debit Card', 'Credit Card', 'Wired Transfer'.

All columns in the `Payments` table are required, and hence should have the `NOT NULL` constraint applied.

### Attendance

the `Attendance` table includes:

`id` which specifies the unique ID for the payment as an `INTEGER`. This column thus has the `PRIMARY KEY` constraint applied.
`member_id` which is the ID of the member as an `INTEGER`. This column thus has the `FOREIGN KEY` constraint applied, referencing the `id` column in the `Members` table to ensure data integrity.
`date` wich specifies the date of the attendance. `TEXT` wich specifies in format of YYYY-MM-DD the entry date of the member,
The default value for the `date` attribute is the current date, as denoted by `DATE NOW`.
`entry_time` wich specifies the time of the attendance. `TEXT` wich specifies in format of HH:MM the time of the attendance.
The default value for the `entry` attribute is the current time, as denoted by `strftime now`.
`exit_time` wich specifies the exit time of the attendance. `TEXT` wich specifies in format of HH:MM the time of the attendance.
The default value for the `exit` attribute is `00:00` as is calculated based of the exit time of the member.

All columns in the `Attendance` table are required, and hence should have the `NOT NULL` constraint applied.

### Employees

the `Employees` table includes:

`id` which specifies the unique ID for the payment as an `INTEGER`. This column thus has the `PRIMARY KEY` constraint applied.
`first_name` which specifies the employee's first name as `TEXT`, given `TEXT` is appropriate for name fields.
`last_name` which specifies the employee's last name. `TEXT` is used for the same reason as `first_name`.
`role` which specifies the employee's role in the gym. `TEXT` is used with constraints to be used only for 'Coach', 'Recepcionist', 'Maintenance', as the only roles within the gym.
`date_birth` wich specifies the employee's date of birth. `TEXT` is used as the date is writing with special characters and numbers.
(e.g 1995-06-27).
`status` wich specifies the status of the member. `TEXT` is used with constraints to be used only for 'Active', 'Not active'.
`phone_number` wich specifies the member´s phone number. `NUMERIC` is appropiate for phone number fields.

All columns in the `Employees` table are required, and hence should have the `NOT NULL` constraint applied.


### Relationships

As detailed by the diagram:

* One member can have zero or many membership records.
Zero, if they have not yet signed up for any membership, and many if they renew or switch memberships over time.
A membership record is linked to one and only one member.
* One membership type (e.g., General, School, Daytime) can be associated with zero or many membership records.
Zero, if no member has selected that type yet, and many if multiple members choose the same type.
Each membership record is tied to one and only one membership type.
* One membership record can generate zero or many payments.
Zero, if the payment has not yet been made, and many if the system allows partial payments or renewals linked to the same record.
Each payment is linked to one and only one membership record.
* One member can log zero or many attendance records.
Zero, if they have never checked in, and many if they attend regularly.
Each attendance record is tied to one and only one member.
* One employee works independently and is not directly linked to other entities in this diagram.
However, employees may be involved in operations such as managing memberships, processing payments, or monitoring attendance, depending on their role (Coach, Receptionist, Maintenance).


## Optimizations

Per the typical queries in `queries.sql`, it is common for users of the database to access all members and memberships entered in the gym. For that reason, indexes are created on the `first_name`, `last_name`, and `role` columns to speed up the identification of members and employees by those columns.

Similarly, it is also common practice for reports of the database to concerned with viewing all memberships "Active" "Not active", therefore an index is created on the `status` column in the `Memberships_records` table to speed up the search.


## Limitations

The current schema assumes individual entries. Everytime a member make a payment or a new employee is hired, all values in `members`,`Membership`, `Memberships_records`, `Payments` and `Employees` must be entered.

## Conclusion

Every member regardless if its new or current member of the gym makes a payment, that member will have an unique ID to identify that purchase.

