<p align="center">
  <img src="assets/Grazioso%20Salvare%20Logo.png" alt="Grazioso Salvare Brand Logo" width="160"/>
</p>

# Search & Rescue Animal Telemetry & Candidate Identification Dashboard
### Full-Stack Geospatial & Reactive Analytics Platform

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)
[![Database](https://img.shields.io/badge/database-MongoDB-green.svg)](https://www.mongodb.com/)
[![Web Framework](https://img.shields.io/badge/framework-Plotly%20Dash-orange.svg)](https://dash.plotly.com/)
[![Design Pattern](https://img.shields.io/badge/architecture-MVC-red.svg)](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)

An interactive, full-stack client/server data analytics dashboard engineered to evaluate, filter, and track animal intake records for search-and-rescue (SAR) deployment suitability. The platform interfaces an authenticated MongoDB document store with a reactive Plotly Dash user interface, coordinating dynamic tabular filtering, aggregate categorical charts, and real-time Leaflet geospatial mapping.

---

## Table of Contents

- [Overview & Scenario](#overview--scenario)
- [System Architecture (MVC Pattern)](#system-architecture-mvc-pattern)
- [Rescue Filter Specifications](#rescue-filter-specifications)
- [Dashboard Features & Verification](#dashboard-features--verification)
- [Repository Structure](#repository-structure)
- [Installation & Setup](#installation--setup)
- [Running the Application](#running-the-application)
- [Technical Challenges & Engineering Solutions](#technical-challenges--engineering-solutions)
- [Tools & Documentation Links](#tools--documentation-links)
- [Author & Acknowledgments](#author--acknowledgments)

---

## Overview & Scenario

Grazioso Salvare identifies dogs that demonstrate strong behavioral and physiological traits for rescue training across three specialized domains:
1. **Water Rescue**
2. **Mountain or Wilderness Rescue**
3. **Disaster Rescue or Individual Tracking**

This dashboard provides rescue coordinators with dynamic query filters, a responsive data table, categorical breed breakdown visualizations, and interactive geolocation mapping centered on the Austin, Texas area.

---

## System Architecture (MVC Pattern)

The application adheres strictly to the **Model-View-Controller (MVC)** software architectural pattern:

```text
       +-------------------------------------------------------+
       |                     CONTROLLER                        |
       |  (Python Dash Callbacks in telemetry-dashboard.ipynb) |
       +-------------------------------------------------------+
              ^                                  |
   Triggers & | Events              Updates Data | & Components
              |                                  v
+--------------------------+       +---------------------------+
|          VIEW            |       |           MODEL           |
| - dash_table.DataTable   |       | - MongoDB (aac database)  |
| - Plotly Express Pie     |       | - CRUD_Python_Module.py   |
| - Dash Leaflet Map       |       |   (AnimalShelter Class)   |
| - dcc.RadioItems Filters |       |                           |
+--------------------------+       +---------------------------+
```

* **Model (`CRUD_Python_Module.py` & MongoDB)**: Encapsulates database connection handling, user authentication, and atomic CRUD operations (`create`, `read`, `update`, `delete`).
* **View (`telemetry-dashboard.ipynb` - Layout)**: Built with `dash_table`, `dash_core_components`, `dash_html_components`, and `dash_leaflet` to render UI widgets, branding, and maps.
* **Controller (`telemetry-dashboard.ipynb` - Callbacks)**: Reactive `@app.callback` decorators listen for user input (radio selections, table pagination, row selection), execute data transformations, and refresh views without page reloads.

---

## Rescue Filter Specifications

Database queries utilize PyMongo atomic operators (`$in`, `$gte`, `$lte`) targeting specific candidate criteria:

| Rescue Domain | Preferred Dog Breeds | Sex | Age Range (Weeks) |
| :--- | :--- | :--- | :--- |
| **Water Rescue** | Labrador Retriever Mix, Chesapeake Bay Retriever, Newfoundland | Intact Female | $26 \le \text{age} \le 156$ |
| **Mountain / Wilderness** | German Shepherd, Alaskan Malamute, Old English Sheepdog, Siberian Husky, Rottweiler | Intact Male | $26 \le \text{age} \le 156$ |
| **Disaster / Tracking** | Doberman Pinscher, German Shepherd, Golden Retriever, Bloodhound, Rottweiler | Intact Male | $20 \le \text{age} \le 300$ |
| **Reset** | All breeds and species in dataset | Any | Any |

---

## Dashboard Features & Verification

### Figure 1: Reset State (Unfiltered View)
![Reset State](assets/screenshot_1_reset.png)
*Figure 1: Initial starting view displaying all 10,007 records, top 10 breed distribution, and initial Austin/Georgetown Leaflet marker.*

### Figure 2: Water Rescue Filter
![Water Rescue State](assets/screenshot_2_water_rescue.png)
*Figure 2: Filter applied for intact female dogs aged 26–156 weeks across Labrador Retriever Mix, Chesapeake Bay Retriever, and Newfoundland breeds.*

### Figure 3: Mountain or Wilderness Rescue Filter
![Mountain Rescue State](assets/screenshot_3_mountain_rescue.png)
*Figure 3: Filter applied for intact male dogs aged 26–156 weeks across German Shepherd, Alaskan Malamute, Old English Sheepdog, Siberian Husky, and Rottweiler breeds.*

### Figure 4: Disaster Rescue or Individual Tracking Filter
![Disaster Rescue State](assets/screenshot_4_disaster_rescue.png)
*Figure 4: Filter applied for intact male dogs aged 20–300 weeks across Doberman Pinscher, German Shepherd, Golden Retriever, Bloodhound, and Rottweiler breeds.*

---

## Repository Structure

```text
my-repository/
├── assets/
│   ├── screenshot_1_reset.png            # Test verification: Default/Reset state
│   ├── screenshot_2_water_rescue.png     # Test verification: Water Rescue filter state
│   ├── screenshot_3_mountain_rescue.png  # Test verification: Mountain Rescue filter state
│   └── screenshot_4_disaster_rescue.png  # Test verification: Disaster Rescue filter state
├── CRUD_Python_Module.py                 # Model: Python class handling authenticated MongoDB CRUD routines
├── telemetry-dashboard.ipynb             # View/Controller: JupyterDash notebook housing layout & callbacks
└── README.md                             # Technical project documentation
```

---

## Installation & Setup

### 1. Prerequisites
* Python 3.8+
* MongoDB Community Server running on `localhost:27017`
* Access to JupyterLab / Jupyter Notebook environment

### 2. Install Required Dependencies
```bash
pip install pymongo pandas numpy plotly dash jupyter-dash dash-leaflet matplotlib
```

### 3. MongoDB Configuration
Ensure the Austin Animal Center dataset is imported into your MongoDB instance under the `aac` database and `animals` collection:
```bash
mongoimport --db aac --collection animals --type csv --headerline --file aac_shelter_outcomes.csv
```

Configure the application user credentials in the MongoDB `admin` database:
```javascript
use admin;
db.createUser({
  user: "aacuser",
  pwd: "<YourPasswordHere>",
  roles: [ { role: "readWrite", db: "aac" } ]
});
```

---

## Running the Application

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/grazioso-salvare-dashboard.git
   cd grazioso-salvare-dashboard
   ```
2. Verify that `CRUD_Python_Module.py` and `Grazioso Salvare Logo.png` reside in the working directory.
3. Open `telemetry-dashboard.ipynb` in JupyterLab.
4. Run all cells sequentially.
5. Access the live dashboard in your web browser:
   * Local: `http://localhost:8050/`
   * Codio Proxy: `https://<your-box-id>-3000.codio.io/proxy/8050/`

---

## Technical Challenges & Engineering Solutions

1. **Cross-Database Authentication Source (`authSource=admin`)**
   * *Problem:* Connection attempts directly to `mongodb://aacuser:pass@localhost:27017/aac` produced `AuthenticationFailed (code 18)`.
   * *Solution:* Explicitly decoupled authentication from the target collection by appending `/?authSource=admin` to the URI connection string in `AnimalShelter.__init__()`.

2. **BSON ObjectId JSON Serialization Failure**
   * *Problem:* Passing raw MongoDB dictionaries into `dash_table.DataTable` triggered `TypeError: Object of type ObjectId is not JSON serializable`.
   * *Solution:* Implemented an early defensive pipeline guard using Pandas:
     ```python
     if '_id' in df.columns:
         df.drop(columns=['_id'], inplace=True)
     ```

3. **Map Marker Desynchronization Across Table Pages (`derived_viewport_*`)**
   * *Problem:* When using `derived_virtual_selected_rows`, navigating to page 2 and clicking row 0 returned index `0`, re-centering the map onto row 0 of page 1 instead of the active page's selection.
   * *Solution:* Changed Callback 4 inputs to consume `derived_viewport_data` and `derived_viewport_selected_rows` for strict page-relative indexing, while retaining `derived_virtual_data` in Callback 2 for accurate full-dataset pie chart aggregations.

4. **Empty Cartesian Axes & Callback Execution Exceptions**
   * *Problem:* Plotly Express occasionally rendered a blank Cartesian grid with x/y coordinate axes (-1 to 4) due to method invocation syntax errors (`dff.empty()` instead of property `dff.empty`).
   * *Solution:* Corrected boolean evaluation, pre-aggregated breed counts via `.value_counts().nlargest(10)`, and pre-seeded `dcc.Graph(figure=init_pie_fig)` in the layout for instantaneous startup rendering.

---

## Tools & Documentation Links

* [MongoDB Documentation](https://www.mongodb.com/docs/)
* [PyMongo Driver Manual](https://pymongo.readthedocs.io/en/stable/)
* [Plotly Dash Python Framework](https://dash.plotly.com/)
* [Plotly Express Pie Charts](https://plotly.com/python/pie-charts/)
* [Dash Leaflet Component Library](https://dash-leaflet.herokuapp.com/)
* [OpenStreetMap Tile API](https://www.openstreetmap.org/)

---

## Engineering Reflection & Systems Design Rationale
### 1. Maintainability, Readability, and Adaptability
Writing modular, maintainable software requires strict separation of concerns. In this architecture, database interaction logic is encapsulated entirely within the `AnimalShelter` class (`CRUD_Python_Module.py`), leaving the view and controller layers in `telemetry-dashboard.ipynb` agnostic of underlying database driver implementations.

* **Advantages of the CRUD Module Abstraction:** Decoupling the data layer ensured that authentication routines, schema sanitization, and PyMongo driver invocations remained isolated. When UI components require data, they interact trhoguh uniform `create()`, `read()`, `update()`, and `delete()` contracts. This prevented database connection leaks, eliminated redundant query code across reactive callbacks, and ensured that changes to database connection parameteres (e.g., configuring `authSource=admin`) required editing only a single class method rather than multiple UI callbacks
* **Future Adaptability & Reuse:** The `AnimalShelter` CRUD module is architected as an independent data-access layer. It can be integrated into headless RESTful APIs (e.g., FastAPI or Flask microservices), automated ETL batch pipelines that ingest daily animal intake logs, or edge-deployed mobile backend services without modifying the core query algorithms.

### 2. The Computer Science Approach to Problem Solving
Approaching client requirements as a computer scientist involved decomposing high-level business goals into precise algorithmic contarsints and optimal data structures:
* **Requirement Decomposition:** Grazioso Salvare required rapid candidate identification based on multi-attirbute physical criteria (breed, age, sex). Rather than pulling the entire dataset into memory and executing imperative loops in Python, the business rules were translated directly into declarative database filter expressions using PyMongo atomic operators (`$in`, `$gte`, `$lte`).

* **Relational vs. Document Paradigms:** In earlier coursework, applications frequently relied on fixing relational schemas requiring multi-table normalization and expensive JOIN operations. This project leveraged MongoDB's document model, treating intake entries as self-contained documents. This provided high read throughput and native handling of semi-structured fields (such as embedded location attributes and dynamic tags) without schema migration overhead.

* **Future Database Strategies:** For future client database designs, key strategies include:
1. *Access Pattern-Driven Schema Design:* Structuring document schemas based primarly on how the front end queries and displays data rather than strictly modeling entity relationships.
2. *Defensive Indexing* Deploying compound indexes (e.g., `{ "breed": 1, "age_upon_outcome_in_weeks": 1, "sex_upon_outcome": 1 }`) to ensure query execution runs sin logarithmic time rather than collection scans as datasets scale. 
3. *Pipeline Decoupling* Using server-side aggregation pipelines (`$match`, `$project`, `$group`) to compute analytical metrics at the database engine level before transmitting payloads over the wire to client applications.

### 3. Impact & Value of Computing in Industry
Computer scientists bridge the gap between massive, unstructured operational data and time-critical decision-making. At its core, computer science is about building automated, scalable, and reliable tools that eliminate human error and manual administrative friction.

For an organization like Grazioso Salvare, rescue operations and training pipelines are resource-contained and time-senstitive. Sifting through tens of thousands of intake records across regional shlters manually would require hundreds of staff hours and increase the likelihood of missing prime rescue candidates. By deploying an automated geospatial telemetry dashboard:
* **Candidate Triage is Instantaneous** Coordinators apply pre-configured rescue profiles with a single click, instantly reducing 10,000+ intake logs to targeted candidate shortlists.
* **Operational Logistics are Streamlined** Real-time geospatial mapping pinpoints exact shelter coordinates, enabling field coordinators to schedule immediate regional logistics and candidate evaluations.
* **Resource Allocation is Optimized** Visual aggregate charts highlight breed demographic distributions across jurisdictions, empowering leadership to direct outreach and rescue funding precisely where suitable candidate populations are concentrated.


---

## Author & Acknowledgments
- **Lead Developer:** Grace Jungclas, MA, AT Ret
- **Core Technologies:** Python, MongoDB, Plotly Dash, Leaflet, Pandas
- **Domain Stakeholders:** Grazioso Salvare & Global Rain
