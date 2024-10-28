# data-sourcing-challenge
This project is part of my Week 5 - Module 5 Challenge for the Rutgers AI Bootcamp.

## Project Details

### Goal
Extract data from NASA’s API spanning **May 2013** to **May 2024** (11 years) from two sources—**CME data** and **GST data**. The objective is to merge the datasets and calculate the average time it takes for a CME event to trigger a corresponding GST event.

[API Documentation](https://api.nasa.gov/)

### Input
- **Source**: NASA API (data on CME and GST events)

### Output
- **File**: collected_data.csv


## Installation

To run this project, ensure you have python installed (version 3.8 or higher).  Follow these steps:

1.  Clone the repository:

```bash
    git clone https://github.com/veenauppalapati/data-sourcing-challenge
```

2.  Navigate to the project directory: 

```bash
    cd data-sourcing-challenge
```

3. Open the project folder in visual studio code

```bash
    code .
```
4. In the project root, create a `.env` file and add your NASA API key:
```bash
NASA_API_KEY = "add your api key here"
```

If you do not have the NASA api_key then go to [NASA API](https://api.nasa.gov/) to register and obtain one.  

5. Run the project: 
Open the `retrieve_data.ipynb` notebook and run the code cells to retrieve, explore, and analyze the dataset.

---

## PROJECT BREAKDOWN

**Request CME data from the NASA API**

- Query the NASA API for CME data using the specified date range:
CME
```
startDate = "2013-05-01"
endDate   = "2024-05-01"

https://api.nasa.gov/DONKI/CME?startDate=yyyy-MM-dd&endDate=yyyy-MM-dd&api_key=DEMO_KEY
```

**CME : Data Cleaning and Transformation**

- Convert the data into a pandas dataframe to proceed with modifications

- **Expand `linkedEvents` Column**: The `linkedEvents` column contains list values.  Expand it so each CME activity ID has a separate row for each linked event.

- Extract the `activityID` from each linked event in the `linkedEvents` column and add it to a new column called `GST_ActivityID`.

- Drop the `linkedEvents` column after extracting the required information.

- Convert columns to their appropriate data types 

- Drop rows with missing `GST_ActivityID` values, as these rows do not have a corresponding GST event.

- Extract the GST event activity ID linked to each CME ID and add it to a new column, `GST_ActivityID`, for reference.

**Request GST data from the NASA API**

Query the NASA API for GST data using the specified date range:
GST
```
startDate = "2013-05-01"
endDate   = "2024-05-01"

https://api.nasa.gov/DONKI/GST?startDate=yyyy-MM-dd&endDate=yyyy-MM-dd&api_key=DEMO_KEY
```

**GST : Data Cleaning and Transformation**

- Convert the data into a pandas dataframe to proceed with modifications

- **Expand `linkedEvents` Column**: The `linkedEvents` column contains list values.  Expand it so each GST activity ID has a separate row for each linked event.

- Extract the `activityID` from each linked event in the `linkedEvents` column and add it to a new column called `CME_ActivityID`.

- Drop the `linkedEvents` column after extracting the required information.

- Convert columns to their appropriate data types 

- Drop rows with missing `CME_ActivityID` values, as these rows do not have a corresponding GST event.

- Extract the CME event activity ID linked to each GST ID and add it to a new column, `CME_ActivityID`, for reference.

**Merge the datasets**

- Merge the datasets using a match on two pairs of columns:

    - Match cmeID from the CME dataset with CME_ActivityID from the GST dataset.

    - Match gstID from the GST dataset with GST_ActivityID from the CME dataset.

**Export the dataset to csv**