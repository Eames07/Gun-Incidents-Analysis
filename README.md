![85d4739f618c4a4415f8ffb49ba980f](https://user-images.githubusercontent.com/97984680/181401656-e781cd2b-3280-47a2-a71a-fdf0d1788bdd.png)


This project aims to analyze gun incidents throughout the United States. 

There is two part to the analysis. The First part is the geographic analysis, and the second is the time analysis.

# Dataset Introduction

Columns(7): 

- ncident_id(incident unique identifier)

- date(recorded date of incident)

- state(US state in which the incident took place)

- city(city within the state where the incident took place)

- address(street address where the incident took place)

- n_killed(number of persons killed during incident)

- n_injured(number of persons injured during incident)

Time span of the data: 2013-2022

Source:https://www.kaggle.com/datasets/emmanuelfwerr/gun-violence-incidents-in-the-usa

Link for this work in my kaggle: https://www.kaggle.com/code/eames07/gun-incidents-usa-eda-data-visualization
# Geographic Analysis

## The Number Of Gun Incidents By States
```python
State_info1 = Gun.state.value_counts().rename_axis('State').reset_index(name='NumberofIncident').sort_values(by=["NumberofIncident"],ascending=True)
State_info1.to_csv('State_info1.csv',index=False)
fig = plt.figure(figsize=(15,12))
plt.barh(State_info1.State,State_info1.NumberofIncident,color="red")
plt.ylabel("States")
plt.xlabel("Number of Incident")
plt.title("Gun Incidents in United States by States")
plt.show()
```
![9ea2faa0c1f783cd1648f22d54b6c00](https://user-images.githubusercontent.com/97984680/181342409-1ecac9c6-e050-46e3-b2f9-22df91c556c2.png)

![ce6a67891a6ea87d22d309f9653bcc0](https://user-images.githubusercontent.com/97984680/181342476-27b023e6-a64f-4e7e-b04e-79f5701e5140.png)


## The Number Of Gun Incidents By Cities
```python
#Gun incidents by cities
City_info1 = Gun.city.value_counts().rename_axis('city').reset_index(name='NumberofIncident').sort_values(by=["NumberofIncident"],ascending=True)
City_info1.to_csv('City_info1.csv',index=False)
City_info1=City_info1.tail(50)
fig = plt.figure(figsize=(15,12))
plt.barh(City_info1.city,City_info1.NumberofIncident,color="red")
plt.ylabel("Cities")
plt.xlabel("Number of Incident")
plt.title("Gun Incidents in United States by Cities")
plt.show()
```
![f2fc1ac034249c241b1dac1b5268beb](https://user-images.githubusercontent.com/97984680/181401772-b5c4d932-e7c8-40ad-a4e6-736ea95336c5.png)
- Note: Red dots are cities have more than 3000 incidents (Only show cities have more than 10 incidents)
![7a5a07199eb2c4d212ec24ff73df251](https://user-images.githubusercontent.com/97984680/181401786-45fca0e2-262d-4e25-b660-4afa03af224c.png)

In terms of the number of accidents by states: Illinois, California, Texas have the most incidents

However, the total number of accidents can't reveal the possibility of getting involved in gun accident. The data of incidents relative to the State population size is required



## The Number Of Gun Incidents Per 10000 Inhabitant Per Year By States
```python
#The Number Of Gun Incident Per 10000 Inhabitant Per Year By State
State_Gun=State_info1.merge(Population,left_on="State",right_on="STATE",suffixes=('_left', '_right'))
State_Gun["IncidentPerInhabitant"]=State_Gun.NumberofIncident/State_Gun.POPESTIMATE2019*10000/8
State_Gun=State_Gun.sort_values(by=["IncidentPerInhabitant"],ascending=True)
fig = plt.figure(figsize=(15,12))
plt.barh(State_Gun.State,State_Gun.IncidentPerInhabitant,color="red")
plt.ylabel("State")
plt.xlabel("Gun Incident in 10000 Inhabitant Per Year")
plt.title("Gun Incident in 10000 Inhabitant Per Year")
plt.show()
```
![df68fc87422cc1f35e06e5c7d8562f0](https://user-images.githubusercontent.com/97984680/181402170-97f66ecd-1357-454f-b1ce-3be0cba77953.png)

![c442d7db9e05756f50ac86a8f2f6dd2](https://user-images.githubusercontent.com/97984680/181402181-c5f0b211-cc28-4d62-80d8-0d62a254e7e9.png)

## The Number Of Gun Incidents Per 10000 Inhabitant Per Year By Cities
```python
#The Number Of Gun Incident Per 10000 Inhabitant Per Year By City
City_info1=City_info1.head(50).sort_values(by=["NumberofIncident"],ascending=False)
City_info1.at[8,"city"]='St. Louis'
Population_City
City_Gun=City_info1.merge(Population_City,left_on="city",right_on="city",suffixes=('_left', '_right'))
City_Gun["IncidentPerInhabitant"]=City_Gun.NumberofIncident/City_Gun.population_2020*10000/8
City_Gun=City_Gun.sort_values(by=["IncidentPerInhabitant"],ascending=True)
fig = plt.figure(figsize=(15,12))
plt.barh(City_Gun.city,City_Gun.IncidentPerInhabitant,color="red")
plt.ylabel("State")
plt.xlabel("Gun Incident in 10000 Inhabitant Per Year")
plt.title("Gun Incident in 10000 Inhabitant Per Year")
plt.show()
```
![438243e4e467505ea07e11be7aa8c9e](https://user-images.githubusercontent.com/97984680/181402216-1009bc98-154b-4271-adc1-4b7ab7b7fafb.png)
St. Louis is the "Vice City" in the United States, unfortunately this is where I am right now

# Time Analysis

## The number of gun incidents by years
```python
Gun['date'] = pd.DatetimeIndex(Gun['date'])
Gun['year'] = Gun.date.dt.year
GunYear=Gun.year.value_counts().rename_axis('Year').reset_index(name='NumberofIncident').sort_values(by=["Year"],ascending=True).drop([8, 9])
#The Number Of Gun Incident From 2014 to 2021
fig = plt.figure(figsize=(12,10))
plt.bar(GunYear.Year,GunYear.NumberofIncident,color="red")
plt.ylabel("Number of Gun Incidents")
plt.xlabel("Year")
plt.title("Number of Gun Incidents")
plt.show()
```
![60f44b01cb3b79ea0a72356a197265d](https://user-images.githubusercontent.com/97984680/181402532-7535a17e-3555-4224-8b13-8eb9dceaaf30.png)

## The number of gun incidents by holidays
```python
#Gun incidents in holidays
Holiday["date"] = pd.DatetimeIndex(Holiday['Date'])
Holiday_Gun=Gun.merge(Holiday,left_on="date",right_on="date",suffixes=('_left', '_right'))
Holiday_Gun1=Holiday_Gun.Holiday.value_counts().rename_axis('Holiday').reset_index(name='NumberofIncident').sort_values(by=["NumberofIncident"],ascending=True)
fig = plt.figure(figsize=(15,12))
plt.barh(Holiday_Gun1.Holiday,Holiday_Gun1.NumberofIncident,color="red")
plt.ylabel("Holiday")
plt.xlabel("NumberofIncident")
plt.title("Number of Incident in Holiday")
plt.show()
```
![9b6e00038886794d68165cd7736c8b3](https://user-images.githubusercontent.com/97984680/181402743-76c309de-5887-4d05-8d44-aae1d4a23cd1.png)
Why there are so many gun incidents in the Labor Day Weekend? Any thought?

## The number of gun incidents by day of week
```python
#Number of Incident group by day of week
Gun["DateofWeek"]= pd.to_datetime(Gun.date, format ="%Y-%m-%d").dt.day_name()
GunDateofWeek=Gun.DateofWeek.value_counts().rename_axis('DateofWeek').reset_index(name='NumberofIncident').sort_values(by=["NumberofIncident"],ascending=False)
fig = plt.figure(figsize=(12,12))
plt.bar(GunDateofWeek.DateofWeek,GunDateofWeek.NumberofIncident,color="red")
plt.ylabel("NumberofIncident")
plt.xlabel("Day of Week")
plt.title("Number of Incident in Weekday")
plt.show()
```
![44032d10ee789ccc8aac8453b3774e1](https://user-images.githubusercontent.com/97984680/181402863-d48d46d0-9727-4312-b167-9fb0547b25c1.png)

# Summary
Total Incidents by States (Top 3): Illinois (35814), California (30745), Texas (30190)

Total Incidents by Cities (Top 3): Chicago (23343), Philadelphia (9879), Texas (7870)

Incidents Per 10000 Inhabitant by States (Top 3): District of Columbia (11.87), Louisiana (4.23), Illinois (3.53)

Incidents Per 10000 Inhabitant by Cities (Top 3): St. Louis(22.14), New Orleans(17.72), Baltimore(16.79)

Incidents by Holiday (Top 3): Labor Day Weekend (2920), New Year’s Day (2061), 4th of July (1955)




