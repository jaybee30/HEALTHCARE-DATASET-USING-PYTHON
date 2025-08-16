# HEALTHCARE-DATASET-USING-PYTHON
## PYTHON SCRIPT
This analysis answers 10 case study questions about a healthcare dataset using python scripts and libraries such as: pandas, matplotlib and seaborn.

EXPLORING THE DATASET
import pandas as pd
healthcare_dataset= pd.read_csv(r"C:\Users\HP 440\Desktop\Health_dataset.csv")
print(healthcare_dataset.head())
-----
healthcare_dataset= pd.read_csv(r"C:\Users\HP 440\Desktop\Health_dataset.csv")
print(healthcare_dataset.head())
------
df= pd.read_csv(r"C:\Users\HP 440\Desktop\Health_dataset.csv")
df
----

IMPORTING LIBRARIES
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

ANSWERING THE CASE STUDY QUESTIONS
1. WHAT IS THE TOTAL NUMBER OF PATIENTS. 
total_records= healthcare_dataset.shape[0]

print('Total number of patients:', total_records)

2. HOW IS THE PATIENT POPULATION DISTRIBUTED BY GENDER?
gender_count = healthcare_dataset['Gender'].value_counts()
print('GENDER DISTRIBUTION')
print(gender_count)
--Visualizing the genderr count
import matplotlib.pyplot as plt
gender_count.plot(kind = 'bar', color=['skyblue', 'lightgreen'])
plt.ylabel('Number of patients')
plt.title('Gender distribution of patients')
plt.legend(loc='upper right')
plt.show()

 3. CASE STUDY 3. PATIENT AGE GROUP BY TOTAL VISIT
healthcare_dataset['Admission Date']= pd.to_datetime(healthcare_dataset['Admission Date'])
healthcare_dataset['Date of Birth'] = pd.to_datetime(healthcare_dataset['Date of Birth'])

healthcare_dataset['Age'] = (healthcare_dataset['Admission Date'] - healthcare_dataset['Date of Birth']).dt.days/365.25
healthcare_dataset['Age'] = healthcare_dataset['Age'].astype(int)
#print(healthcare_dataset['Age'])

#next is to create a bin and label to create an age group
bins= (0, 20, 40, 60, 80, 100)
labels = ('0-20', '21-40', '41-60', '61-80', '81-100')

healthcare_dataset['Age Group']= pd.cut(healthcare_dataset['Age'], bins=bins, labels=labels, right=False)
healthcare_dataset['Age Group']

age_count = healthcare_dataset['Age Group'].value_counts()
#age_count

#visualize the age group distribution
ax=age_count.plot(kind = 'bar', color = 'lightgreen')
plt.title('AGE GROUP DISTRIBUTION')
#data label code
for bar in ax.patches:
    height= bar.get_height()
    ax.annotate(
        int(height),
        xy= (bar.get_x() + bar.get_width()/2, height),
        xytext = (0, 5),
        textcoords = "offset points",
        ha= 'center',
        va ='bottom',
    
    )

#CASE STUDY 4: WHICH DISEASES ARE MOST COMMONLY DIAGNOSED AMONG THE PATIENTS

disease_count= healthcare_dataset['Disease'].value_counts()
print(disease_count)
#plot a visualization for this result
ax= disease_count.plot(kind = 'bar', color = ['red'])
Top 3 diseases---
top3 = healthcare_dataset['Disease'].value_counts().nlargest(3)
print(top3)

CASE STUDY 5: Are certain diseases more prevalent in one gender compared to the other ? 
gender_disease = pd.crosstab(healthcare_dataset['Disease'], healthcare_dataset['Gender'])
print("Prevalency of diseases among genders")
print(gender_disease)

CASE STUDY 6: Calculate the duration of each patient stay and create a group to show numbers of each patients in each group
df['Admission Date'] = pd.to_datetime(df['Admission Date'])
df['Discharge Date'] = pd.to_datetime(df['Discharge Date'])

df['Stay Duration'] = (df['Discharge Date'] - df['Admission Date']).dt.days
print(df[['Admission Date', 'Discharge Date','Stay Duration']])
# next is to create bin and label to group the stay duration  
bins = [-1,0,3,7,14, float('inf')]
labels = ["Same Day", "1-3 Days", "4-7 Days", "8-14 Days,", "Above 20 Days"]
df['Stay Bucket'] = pd.cut(df['Stay Duration'], bins=bins, labels=labels, right=True)
print(df['Stay Bucket'])
## number of patients in each group 
total_group_count = df['Stay Bucket'].value_counts()
print(total_group_count)
Visualizing patients in each group ---
total_group_count.plot (kind = 'bar', color =['#026773'])
plt.title('NUMBER OF PATIENTS IN RESPECT TO LENGTH OF STAY')


CASE STUDY 7: FOR ANY RECORDS WITH A RECORDED CAUSE OF DEATH, ANALYZE PATTERN TO IDENTIFY RISK FACTOR
## filter records where the cause of death is not empty using dropna and passing the column name into subset
death_cause = df.dropna(subset='Cause of Death')
# count the number of records that has a value under cause of death column
count_cause = death_cause['Cause of Death'].value_counts()
print('Most common cause of death')
print(count_cause)

--visualize the cause of death
import matplotlib.pyplot as plt
#plt.figure is used to customize the height and width of chart
plt.figure (figsize=(12,4))
count_cause.plot( kind = 'bar', color='red')
#plt.xticks and rotation is used when the labels in the x axis is too long and you want it slanted
plt.xticks(rotation = 45)
plt.title ('Cause of death')

CASE STUDY 8 WHAT ARE THE PERCENTAGE OF THE FOLLOWING PATIENTS: DECEASED, UNDER TREATMENT AND RECOVERED PATIENTS?
total_patient = df.shape[0]
print(total_patient)
### total death patients

total_death =(df['Treatment Status'].str.lower() =='deceased').sum()
print(total_death )
#death_rate
death_rate = (total_death / total_patient) * 100 if total_patient >0 else 0
print(death_rate)
print(f"Total Patients:", total_patient)
print(f"Total Death:", total_death)
print(f"Death Rate:", death_rate)

### recovered patients
total_recovered =(df['Treatment Status'].str.lower() =='recovered').sum()
print(total_recovered )
#total_recovered =(df['Treatment Status'].str.upper() =='RECOVERED').sum()
#print(total_recovered )

#recovered_rate
recovered_rate = (total_recovered / total_patient) * 100 if total_patient >0 else 0
print(recovered_rate)

print(f"Total Patients:", total_patient)
print(f"Total Recovered", total_recovered)
print(f"Recovered Rate:", recovered_rate)
## Under treatment patients
under_treatment = 100 - (recovered_rate + death_rate) 
print(under_treatment)
print(f"Death Rate:", death_rate)
print(f"Recovered Rate:", recovered_rate)
print(f"Under Treatment:", under_treatment)
##visualize the percentage deceased, recovered and under treatment patients
import matplotlib.pyplot as plt
labels = ['Recovered', 'Deceased', 'Under treatment']
sizes = [recovered_rate, death_rate, under_treatment]
colors= ['green', 'red', 'blue']
plt. Figure(figsize = (12,10))
plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors, startangle=140, wedgeprops={'edgecolor': 'white', 'linewidth':2})
plt.gca().add_artist(plt.Circle((0,0), 0.70, fc='white'))
plt.title('Treatment Status')
plt.show()

CASE STUDY 9 WHAT ARE THE PEAK DAYS OF THE WEEK ON A MONTHLY BASES FOR ADMISSIONS AND DISCHARGES

#HEAT MAP FOR PEAK ADMISSION PERIOD
# CONVERT ADMISSION DATE TO PROPER DATE FORMAT
df['Admission Date'] = pd.to_datetime(df['Admission Date'], errors="coerce")

# Extract the short day and month name
df['Admission Month']= df['Admission Date'].dt.strftime('%b')
df['Admission Day']= df['Admission Date'].dt.strftime('%a')

# DEFINE PROPER ORDER FOR MONTH AND ABBREVIATEED DAYS (SORTING)
month_order= [
    'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
    'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'
]

day_order = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'] 

# Create a pivot table
admission_heatmap = df.pivot_table(
    index = 'Admission Day',
    columns = 'Admission Month',
    aggfunc = 'size',
    fill_value = 0
)

admission_heatmap_sorted= admission_heatmap.reindex(index= day_order, columns = month_order)

PLOTTING THE HEATMAP
plt.figure(figsize=(12,6))
sns.heatmap(admission_heatmap_sorted, annot= True, fmt= "d", cmap="YlOrRd")
plt.title('Admission heatmap', fontsize=20)
plt.show()
                                    
# HEATMAP FOR PEAK DISCHARGE PERIOD
df['Discharge Date'] = pd.to_datetime(df['Discharge Date'], errors="coerce")

# Extract the short day and month name
df['Discharge Month']= df['Discharge Date'].dt.strftime('%b')
df['Discharge Day']= df['Discharge Date'].dt.strftime('%a')

# DEFINE PROPER ORDER FOR MONTH AND ABBREVIATEED DAYS (SORTING)
month_order= [
    'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
    'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'
]

day_order = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'] 

# Create a pivot table
discharge_heatmap = df.pivot_table(
    index = 'Discharge Day',
    columns = 'Discharge Month',
    aggfunc = 'size',
    fill_value = 0
)

discharge_heatmap_sorted= discharge_heatmap.reindex(index= day_order, columns = month_order)

# PLOTTING THE HEATMAP
import seaborn as sns
plt.figure(figsize=(12,6))
sns.heatmap(discharge_heatmap_sorted, annot= True, fmt= "d", cmap="Greens")
plt.title('Discharge heatmap', fontsize=20)
plt.show()   

CASE STUDY 10 WHAT IS THE TOTAL NUMBER OF PATIENTS ADMITTED DURING THE WEEK, MONTH AND YEAR? ADD A FILTER TO FILTER THE MONTH BY YEAR
#start by creating a new column admission day
df['Admission Day']= df['Admission Date'].dt.strftime('%a')
df

#group data by day name and count admission
admission_by_day = df.groupby('Admission Day').size()
admission_by_day

#sort the name of day from mon to sunday. fill_value=0 will attach 0 to any day without record

day_order = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
admission_by_day = admission_by_day.reindex(day_order, fill_value=0)
admission_by_day
-- visualization
admission_by_day.plot(kind='bar', color='#FF6E00', figsize=(10,4))
plt.title('Admisssion by day of the week')
admission_by_day
plt.xticks(rotation=5)
plt.show()

#THSI CODE WILL TAKE A YEAR TO BE FILTERED AS SELECTED YEAR, THEN RETURN RESULTS FOR TOTAL PATIENTS ADMITTED WITHIN THAT MONTH IN A SELECTED YEAR
#Then the f string will take in the selected year with proper title to display
selected_year = 2022
df_year = df[df['Admission Date'].dt.year == selected_year].copy()

# extracting the month number 
df_year['Admission Month'] = df['Admission Date'].dt.month


# extracting the full month name (January, February etc....)
df_year['Month Name'] = df_year['Admission Date'].dt.month_name()


#Total admission on a monthly basis
monthly_admission =(
    df_year
    .groupby(['Admission Month','Month Name'])
    .size()
    .reset_index(name = 'Total_patient')
    .sort_values('Admission Month')
)
print(f"Total patients admitted in the year {selected_year}")
monthly_admission 

-- Visualization with donut chart
import matplotlib.pyplot as plt
#
plt.figure(figsize = (15,5))

plt.plot(monthly_admission['Month Name'], monthly_admission['Total_patient'],
         marker ='1',  linestyle = '-', color = 'orange' 
)

print(f"Total patients admitted in the year {selected_year}")

## END OF THE CASE STUDY. VOILA 👏##

RESULTS

<img width="896" height="568" alt="Screenshot (17)" src="https://github.com/user-attachments/assets/17165c12-06ba-4cc5-a412-245edb0decfd" />
<img width="1195" height="531" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/e0ab34e4-9f9e-4ba7-9f51-ebb5969c4bff" />
<img width="1275" height="543" alt="Screenshot (19)" src="https://github.com/user-attachments/assets/597def79-3f2d-4b81-9f30-05bf91181519" />
<img width="1366" height="640" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/bd16ff99-444a-4ff3-a9be-0ff727b09427" />
<img width="1366" height="411" alt="Screenshot (21)" src="https://github.com/user-attachments/assets/b3eb9515-62c3-4946-be7b-6de20d486023" />
<img width="1366" height="509" alt="Screenshot (22)" src="https://github.com/user-attachments/assets/c2abbeae-1eb0-4e44-b6f0-1efbe67154bc" />
<img width="1366" height="452" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/7a2c4bb7-9ee8-4cba-a0a7-134f4ec76186" />
<img width="1366" height="520" alt="Screenshot (24)" src="https://github.com/user-attachments/assets/ea4df07e-8860-4862-bbcc-d07f0196e8ae" />
<img width="1366" height="548" alt="Screenshot (25)" src="https://github.com/user-attachments/assets/962d5ffc-06c5-40b2-887c-852614015cc0" />
<img width="1366" height="542" alt="Screenshot (26)" src="https://github.com/user-attachments/assets/c462019f-f386-409a-b433-c3663b091fe8" />
<img width="1366" height="419" alt="Screenshot (27)" src="https://github.com/user-attachments/assets/0a70c414-7660-48df-8af9-e4717bd0a6fe" />
<img width="1366" height="427" alt="Screenshot (29)" src="https://github.com/user-attachments/assets/dab0cd2c-990c-4989-89d8-070ed52d2ade" />
<img width="1366" height="540" alt="Screenshot (30)" src="https://github.com/user-attachments/assets/9836676f-804f-489d-afdd-80f56cf20532" />
<img width="1366" height="556" alt="Screenshot (31)" src="https://github.com/user-attachments/assets/307eb80b-519e-4380-85eb-2210ab2a07cf" />
<img width="1366" height="419" alt="Screenshot (32)" src="https://github.com/user-attachments/assets/851482e2-3a38-44be-a13a-872211e2cb69" />
<img width="1366" height="423" alt="Screenshot (33)" src="https://github.com/user-attachments/assets/b03628f5-fcf7-40e4-a3ce-4aa34294557a" />















