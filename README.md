[README.txt](https://github.com/user-attachments/files/23697672/README.txt)
# Airline Booking Application (ABAP)

This project is an SAP ABAP application that simulates a simplified airline booking system. It allows a registered passenger to log in, search for available flights, view seat availability, choose a travel class, purchase a ticket, and generate a SmartForm invoice. The program is built as a classical dynpro (module pool–style) application using multiple screens, ALV Grid, custom DDIC tables, and SAP standard function modules.

---

## Features

### **1. Passenger Login**

* User authentication based on passenger ID and password.
* Validation against the custom table `ZZZ_PASS_TAB`.
* Navigation to the main flight search interface upon successful login.

### **2. Flight Search**

* Dynamic dropdowns for departure and arrival cities.
* Cities retrieved from the custom table `ZFLIGHTSCHEDULE`.
* Automatic filtering of arrival cities based on selected departure city.
* Transition to the flight list screen after selecting route parameters.

### **3. Flight List Display (ALV Grid)**

* Flights retrieved from a join of `ZFLIGHTSCHEDULE` and `ZZZ_PRICELIST`.
* ALV field catalog generated dynamically using `LVC_FIELDCATALOG_MERGE`.
* Custom added columns for:

  * Business class seat status: available/occupied.
  * Economy class seat status: available/occupied.
* Double-click event handler implemented via local class `LCL_EVENT_HANDLER`.
* Seat occupancy dynamically calculated based on existing records in `ZZZ_RESERVATIONS`.

### **4. Ticket Class Selection**

* Double-clicking a flight opens a pop-up to choose:

  * Business class
  * Economy class
* Seat availability validated in real time.
* Stores selected class, price, and currency for final ticket generation.

### **5. Ticket Purchase & Invoice Generation**

* Passenger and flight details retrieved automatically from DDIC tables.
* SmartForm `Z_INVOICE_TICKET` generated via:

  * `SSF_FUNCTION_MODULE_NAME`
  * SmartForm function module execution.
* Reservation saved to table `ZZZ_RESERVATIONS`.

---

## Technologies & SAP Components Used

### **ABAP Dynpro (Classical Screen Programming)**

* Multiple screens (0100, 0200, 0300, 0400)
* PBO/PAI modules for screen logic
* GUI containers and ALV controls

### **ABAP Objects**

* Local class `LCL_EVENT_HANDLER` for ALV double-click event handling

### **Custom DDIC Objects**

The application relies on several custom objects:

#### **Tables**

* **ZZZ_PASS_TAB** – Passenger data
* **ZFLIGHTSCHEDULE** – Flight schedule (cities, dates, times, plane no.)
* **ZZZ_PRICELIST** – Prices and seat limits per class
* **ZZZ_RESERVATIONS** – Saved ticket reservations

#### **Structures**

* **ZZZ_STR_FLIGHT_DATA** – Unified structure used for ALV display and data handling

### **SAP Standard Components**

* **CL_GUI_ALV_GRID** – ALV display of available flights
* **CL_GUI_CUSTOM_CONTAINER** – Container for ALV embedding
* **VRM_SET_VALUES** – Dropdown list population
* **POPUP_TO_CONFIRM** – Seat class selection dialog
* **SSF_FUNCTION_MODULE_NAME** – SmartForm retrieval
* **SmartForms** – Ticket/invoice printing

---

## Architecture & Flow Overview

1. **Screen 0100 – Login**

   * Inputs: Passenger ID, password
   * Validation against `ZZZ_PASS_TAB`

2. **Screen 0200 – Route Selection**

   * Dropdowns dynamically populated via `VRM_SET_VALUES`
   * Input: Departure city, Arrival city

3. **Screen 0300 – Flight Selection**

   * ALV grid showing all matching flights
   * Double-click opens class selection popup

4. **Screen 0400 – Summary & Purchase**

   * Displays full ticket details
   * Generates SmartForm invoice
   * Inserts reservation into `ZZZ_RESERVATIONS`

---

## Additional Notes

* All seat availability calculations are performed in real time based on current reservations.
* Custom columns in the ALV grid reflect **total seats / occupied seats**.
* The program follows a modular dynpro structure, ensuring clean separation of UI logic and backend operations.
* Designed for training, educational, or demonstration purposes in SAP ABAP environments.

---

