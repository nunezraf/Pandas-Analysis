
## CONCLUSIONS FROM THE DATA 

###  1-Schools with higher budget per students have the lowest passing overall percentages. 
###  2-Students enrrolled in charter type schools have better performance than students in distric type schools.
###  3-Averages for math scores are lower than average reading scores consisntly throughout the entire data.



```python
# Importing python libraries 
import pandas as pd
import numpy as np 
```


```python
# Loading the data for analysis 

Schools_Data_df = "resources/schools_complete.csv"
Students_Data_df= "resources/students_complete.csv"
```


```python
#read school and students data 

schools_df= pd.read_csv(Schools_Data_df)
students_df= pd.read_csv(Students_Data_df)

schools_df.rename(columns = {'name':'school'}, inplace = True)
schools_df.head()


merged_df = students_df.merge(schools_df, how = 'left', on = 'school')
#students_df.head()
```


```python
#renames for merge
schools_df.rename(columns = {'name':'school'}, inplace = True)



```

## District Summary Table


```python
#merging the two data sets by using school names as common key 

# school_complete_data = pd.merge(schools_df,student_df, on=["school_name"], how= "left")
# school_complete_data.head()
# complete_data= schools_df.merge(students_df, on='school', how='left')

# merged_df = pd.merge(schools_df,students_df, on = "name", how="left")
# merged_df.head()

school_data_complete = pd.merge(students_df, schools_df, how="left", on="school")


```


```python
total_schools = len(schools_df)
total_schools
```




    15




```python
#total numeber of students 
total_students = len(school_data_complete)
total_students 
```




    39170




```python
#total number of schools
total_school_budget= schools_df['budget'].sum(numeric_only= False)
total_school_budget
```




    24649428




```python
#math score average
math_avg = students_df['math_score'].mean()
np.round(math_avg, decimals=2)
```




    78.99




```python
#reading score average 
reading_avg = students_df['reading_score'].mean()
np.round(reading_avg, decimals=2)
```




    81.88




```python
# #math score passing percent

passing_math = students_df.loc[students_df['math_score'] >= 70]['math_score'].count()
passing_math
```




    29370




```python
#calculatig math passing percentage 
percent_passing_math = (passing_math/total_students)*100
np.round(percent_passing_math, decimals=2)
```




    74.98




```python
# calculating number of students passing reading
passing_reading = students_df.loc[students_df['reading_score'] >= 70]['reading_score'].count()
passing_reading 
```




    33610




```python
#calculating reading passing percentage
percent_passing_reading= np.round((passing_reading/total_students)*100,2)
percent_passing_reading
```




    85.81




```python
#calculating_overall_passing_percentage
overall_passing_rate= np.round((percent_passing_reading + percent_passing_math)/2,2)
overall_passing_rate
```




    80.4




```python
#constructing the new data_frame using dictionaries 
District_df = pd.DataFrame({
                 "Total Schools" : [total_schools],
                 'Total Students':[total_students],
                 "Total Budget" :[total_school_budget],
                 "Average Math Score" :[math_avg],
                 "Average Reading Score" :[reading_avg],
                 "% Passing Math" :[percent_passing_math],
                 "% Passing Reading" :[percent_passing_reading],
                 "Overall Passing Rate" :[overall_passing_rate] }, columns = ["Total Students", "Total Schools", "Total Budget", "Average Math Score", "Average Reading Score","% Passing Math", "% Passing Reading", "Overall Passing Rate"])

District_df.style.format({'Average Math Score': '{:.1f}', 
                         'Average Reading Score': '{:.1f}', 
                        '% Passing Math': '{:.0f}%', 
                         '% Passing Reading':'{:.0f}%', 
                        'Overall Passing Rate': '{:.2f}%',
                         'Total Budget': "${:,}", 
                          'Total Students': "{:,}",})

```




<style  type="text/css" >
</style>  
<table id="T_5c1f6b5e_6abb_11e8_a84a_8c859026d06c" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Students</th> 
        <th class="col_heading level0 col1" >Total Schools</th> 
        <th class="col_heading level0 col2" >Total Budget</th> 
        <th class="col_heading level0 col3" >Average Math Score</th> 
        <th class="col_heading level0 col4" >Average Reading Score</th> 
        <th class="col_heading level0 col5" >% Passing Math</th> 
        <th class="col_heading level0 col6" >% Passing Reading</th> 
        <th class="col_heading level0 col7" >Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_5c1f6b5e_6abb_11e8_a84a_8c859026d06clevel0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_5c1f6b5e_6abb_11e8_a84a_8c859026d06crow0_col0" class="data row0 col0" >39,170</td> 
        <td id="T_5c1f6b5e_6abb_11e8_a84a_8c859026d06crow0_col1" class="data row0 col1" >15</td> 
        <td id="T_5c1f6b5e_6abb_11e8_a84a_8c859026d06crow0_col2" class="data row0 col2" >$24,649,428</td> 
        <td id="T_5c1f6b5e_6abb_11e8_a84a_8c859026d06crow0_col3" class="data row0 col3" >79.0</td> 
        <td id="T_5c1f6b5e_6abb_11e8_a84a_8c859026d06crow0_col4" class="data row0 col4" >81.9</td> 
        <td id="T_5c1f6b5e_6abb_11e8_a84a_8c859026d06crow0_col5" class="data row0 col5" >75%</td> 
        <td id="T_5c1f6b5e_6abb_11e8_a84a_8c859026d06crow0_col6" class="data row0 col6" >86%</td> 
        <td id="T_5c1f6b5e_6abb_11e8_a84a_8c859026d06crow0_col7" class="data row0 col7" >80.40%</td> 
    </tr></tbody> 
</table> 




```python
new_schools_df = schools_df.set_index(["school"])

#Grouping by school to analyzed the data by school 
groupby_school = students_df.groupby("school")

# #school type 
sch_type = new_schools_df["type"]

# #total students 
sch_students= groupby_school['name'].count()

# # Total School Budget 
sch_budget= new_schools_df["budget"]



# # #Per Student Budget 
per_student_bdgt = schools_df.set_index('school')['budget']/schools_df.set_index('school')['size']




```

## SCHOOL  SUMMARY TABLE


```python




# Create the School Summary dataframe by intializing the starting value
School_Summary_DF=pd.DataFrame({"School Name": schools_df["school"],
                               "School Type":schools_df["type"],
                               "Budget":schools_df["budget"],
                               "Total Students":schools_df["size"]})
# Find the count of student who passed math and reading separately in each school
Count_passing_math = school_data_complete.loc[(students_df['math_score'] >= 70)].groupby(["school"]).count()["math_score"]
Count_passing_reading = school_data_complete.loc[(students_df['reading_score'] >= 70)].groupby(["school"]).count()["reading_score"]
Count_passing_overall = school_data_complete.loc[(school_data_complete['math_score'] >=70) 
                                           & (school_data_complete['reading_score'] >=70)].groupby(["school"]).count()["Student ID"]
# Create a temporary dataframe to store the calculated values
temp_df=pd.DataFrame({"Avg Math Score": school_data_complete.mean()["math_score"],
                "Avg Reading Score": school_data_complete.mean()["reading_score"],
                "Count of Passing Math": Count_passing_math,
                "Count of Passing Reading":Count_passing_reading,
                "Count of Passing Overall":Count_passing_overall})
# Reset the index so school becomes a column
temp_df.reset_index(inplace=True)
# Rename the column school before merging
temp_df = temp_df.rename(columns={"school":"School Name"})
# Merge both the Summary dataframe and the temporary dataframe on School Name
School_Summary_DF=pd.merge(School_Summary_DF,temp_df,how="outer",on="School Name")
# Calculate the remaining values
School_Summary_DF["% Passing Math"]=(School_Summary_DF["Count of Passing Math"]/School_Summary_DF["Total Students"])*100
School_Summary_DF["% Passing Reading"]=(School_Summary_DF["Count of Passing Reading"]/School_Summary_DF["Total Students"])*100
School_Summary_DF["% Passing Overall"]=(School_Summary_DF["Count of Passing Overall"]/School_Summary_DF["Total Students"])*100
# Drop the columns after computation
School_Summary_DF.drop(["Count of Passing Math","Count of Passing Reading","Count of Passing Overall"],axis=1,inplace=True)
# Set the index as School Name
School_Summary_DF.set_index("School Name",inplace=True)

School_Summary_DF.head()



```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Budget</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
    </tr>
    <tr>
      <th>School Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>1910635</td>
      <td>District</td>
      <td>2917</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>53.513884</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>1884411</td>
      <td>District</td>
      <td>2949</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>53.204476</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>1056600</td>
      <td>Charter</td>
      <td>1761</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>89.892107</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>3022020</td>
      <td>District</td>
      <td>4635</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>53.527508</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>917500</td>
      <td>Charter</td>
      <td>1468</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>90.599455</td>
    </tr>
  </tbody>
</table>
</div>



## TOP 5 PERFORMING SCHOOLS 


```python
#top 5 Peforming Schools 

top_5 = School_Summary_DF.sort_values(by="% Passing Overall",ascending=False)
top_5.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Budget</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
    </tr>
    <tr>
      <th>School Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Cabrera High School</th>
      <td>1081356</td>
      <td>Charter</td>
      <td>1858</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>91.334769</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>1043130</td>
      <td>Charter</td>
      <td>1635</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>90.948012</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>917500</td>
      <td>Charter</td>
      <td>1468</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>90.599455</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>1319574</td>
      <td>Charter</td>
      <td>2283</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>90.582567</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>585858</td>
      <td>Charter</td>
      <td>962</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>90.540541</td>
    </tr>
  </tbody>
</table>
</div>



## BOTTOM 5 PERFORMING SCHOOLS 


```python
#bottom 5 

bottom_5 = School_Summary_DF.sort_values(by = '% Passing Overall', ascending= True)
bottom_5.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Budget</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Avg Math Score</th>
      <th>Avg Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
    </tr>
    <tr>
      <th>School Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <td>2547363</td>
      <td>District</td>
      <td>3999</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>52.988247</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>1884411</td>
      <td>District</td>
      <td>2949</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>53.204476</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>1910635</td>
      <td>District</td>
      <td>2917</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>53.513884</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>3022020</td>
      <td>District</td>
      <td>4635</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>53.527508</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>3094650</td>
      <td>District</td>
      <td>4761</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>53.539172</td>
    </tr>
  </tbody>
</table>
</div>



## MATH SCORES BY GRADE


```python
#Math Scores by Grade 


#creates grade level average math scores for each school 
#creates grade level average math scores for each school 
ninth_grade_math = students_df.loc[students_df['grade'] == '9th'].groupby('school')['math_score'].mean()
tenth_grade_math = students_df.loc[students_df['grade']== '10th'].groupby('school')['math_score'].mean()
eleventh_grade_math= students_df.loc[students_df['grade']== '11th'].groupby('school')['math_score'].mean()
twelveth_grade_math= students_df.loc[students_df['grade']=='12th'].groupby('school')['math_score'].mean()

math_scores_by_grade = pd.DataFrame({ '9th': ninth_grade_math,
                                     '10th': tenth_grade_math, 
                                     '11th':eleventh_grade_math, 
                                     '12th':twelveth_grade_math})

math_scores_by_grade= math_scores_by_grade[['9th', '10th', '11th', '12th']]
math_scores_by_grade.index.name = "School Name"

#format
math_scores_by_grade.style.format({'9th': '{:.1f}', 
                             "10th": '{:.1f}', 
                             "11th": "{:.1f}", 
                             "12th": "{:.1f}"})

```




<style  type="text/css" >
</style>  
<table id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06c" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >9th</th> 
        <th class="col_heading level0 col1" >10th</th> 
        <th class="col_heading level0 col2" >11th</th> 
        <th class="col_heading level0 col3" >12th</th> 
    </tr>    <tr> 
        <th class="index_name level0" >School Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow0_col0" class="data row0 col0" >77.1</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow0_col1" class="data row0 col1" >77.0</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow0_col2" class="data row0 col2" >77.5</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow0_col3" class="data row0 col3" >76.5</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow1_col0" class="data row1 col0" >83.1</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow1_col1" class="data row1 col1" >83.2</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow1_col2" class="data row1 col2" >82.8</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow1_col3" class="data row1 col3" >83.3</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow2_col0" class="data row2 col0" >76.4</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow2_col1" class="data row2 col1" >76.5</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow2_col2" class="data row2 col2" >76.9</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow2_col3" class="data row2 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow3_col0" class="data row3 col0" >77.4</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow3_col1" class="data row3 col1" >77.7</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow3_col2" class="data row3 col2" >76.9</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow3_col3" class="data row3 col3" >76.2</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow4_col0" class="data row4 col0" >82.0</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow4_col1" class="data row4 col1" >84.2</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow4_col2" class="data row4 col2" >83.8</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow4_col3" class="data row4 col3" >83.4</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow5_col0" class="data row5 col0" >77.4</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow5_col1" class="data row5 col1" >77.3</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow5_col2" class="data row5 col2" >77.1</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow5_col3" class="data row5 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow6_col0" class="data row6 col0" >83.8</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow6_col1" class="data row6 col1" >83.4</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow6_col2" class="data row6 col2" >85.0</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow6_col3" class="data row6 col3" >82.9</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow7_col0" class="data row7 col0" >77.0</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow7_col1" class="data row7 col1" >75.9</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow7_col2" class="data row7 col2" >76.4</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow7_col3" class="data row7 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow8_col0" class="data row8 col0" >77.2</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow8_col1" class="data row8 col1" >76.7</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow8_col2" class="data row8 col2" >77.5</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow8_col3" class="data row8 col3" >76.9</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow9_col0" class="data row9 col0" >83.6</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow9_col1" class="data row9 col1" >83.4</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow9_col2" class="data row9 col2" >84.3</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow9_col3" class="data row9 col3" >84.1</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow10_col0" class="data row10 col0" >76.9</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow10_col1" class="data row10 col1" >76.6</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow10_col2" class="data row10 col2" >76.4</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow10_col3" class="data row10 col3" >77.7</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow11_col0" class="data row11 col0" >83.4</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow11_col1" class="data row11 col1" >82.9</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow11_col2" class="data row11 col2" >83.4</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow11_col3" class="data row11 col3" >83.8</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow12_col0" class="data row12 col0" >83.6</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow12_col1" class="data row12 col1" >83.1</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow12_col2" class="data row12 col2" >83.5</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow12_col3" class="data row12 col3" >83.5</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow13_col0" class="data row13 col0" >83.1</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow13_col1" class="data row13 col1" >83.7</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow13_col2" class="data row13 col2" >83.2</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow13_col3" class="data row13 col3" >83.0</td> 
    </tr>    <tr> 
        <th id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06clevel0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow14_col0" class="data row14 col0" >83.3</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow14_col1" class="data row14 col1" >84.0</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow14_col2" class="data row14 col2" >83.8</td> 
        <td id="T_6378f4c6_6abb_11e8_9f8b_8c859026d06crow14_col3" class="data row14 col3" >83.6</td> 
    </tr></tbody> 
</table> 



## READING SCORES BY GRADE 


```python
#Reading Scores by Grade 
#creates grade level average reading scores for each school 

ninth_grade_reading = students_df.loc[students_df['grade'] == '9th'].groupby('school')["reading_score"].mean()
tenth_grade_reading = students_df.loc[students_df['grade']== '10th'].groupby('school')["reading_score"].mean()
eleventh_grade_reading= students_df.loc[students_df['grade']== '11th'].groupby('school')["reading_score"].mean()
twelveth_grade_reading= students_df.loc[students_df['grade']=='12th'].groupby('school')["reading_score"].mean()

reading_scores_by_grade = pd.DataFrame({ '9th': ninth_grade_reading,
                                     '10th': tenth_grade_reading, 
                                     '11th':eleventh_grade_reading, 
                                     '12th':twelveth_grade_reading})

reading_scores_by_grade= reading_scores_by_grade[['9th', '10th', '11th', '12th']]
reading_scores_by_grade.index.name = "School Name"

#style
reading_scores_by_grade.style.format({'9th': '{:.1f}', 
                             "10th": '{:.1f}', 
                             "11th": "{:.1f}", 
                             "12th": "{:.1f}"})

```




<style  type="text/css" >
</style>  
<table id="T_63818512_6abb_11e8_81d2_8c859026d06c" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >9th</th> 
        <th class="col_heading level0 col1" >10th</th> 
        <th class="col_heading level0 col2" >11th</th> 
        <th class="col_heading level0 col3" >12th</th> 
    </tr>    <tr> 
        <th class="index_name level0" >School Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow0_col0" class="data row0 col0" >81.3</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow0_col1" class="data row0 col1" >80.9</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow0_col2" class="data row0 col2" >80.9</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow0_col3" class="data row0 col3" >80.9</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow1_col0" class="data row1 col0" >83.7</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow1_col1" class="data row1 col1" >84.3</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow1_col2" class="data row1 col2" >83.8</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow1_col3" class="data row1 col3" >84.3</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow2_col0" class="data row2 col0" >81.2</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow2_col1" class="data row2 col1" >81.4</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow2_col2" class="data row2 col2" >80.6</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow2_col3" class="data row2 col3" >81.4</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow3_col0" class="data row3 col0" >80.6</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow3_col1" class="data row3 col1" >81.3</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow3_col2" class="data row3 col2" >80.4</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow3_col3" class="data row3 col3" >80.7</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow4_col0" class="data row4 col0" >83.4</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow4_col1" class="data row4 col1" >83.7</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow4_col2" class="data row4 col2" >84.3</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow4_col3" class="data row4 col3" >84.0</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow5_col0" class="data row5 col0" >80.9</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow5_col1" class="data row5 col1" >80.7</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow5_col2" class="data row5 col2" >81.4</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow5_col3" class="data row5 col3" >80.9</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow6_col0" class="data row6 col0" >83.7</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow6_col1" class="data row6 col1" >83.3</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow6_col2" class="data row6 col2" >83.8</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow6_col3" class="data row6 col3" >84.7</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow7_col0" class="data row7 col0" >81.3</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow7_col1" class="data row7 col1" >81.5</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow7_col2" class="data row7 col2" >81.4</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow7_col3" class="data row7 col3" >80.3</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow8_col0" class="data row8 col0" >81.3</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow8_col1" class="data row8 col1" >80.8</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow8_col2" class="data row8 col2" >80.6</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow8_col3" class="data row8 col3" >81.2</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow9_col0" class="data row9 col0" >83.8</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow9_col1" class="data row9 col1" >83.6</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow9_col2" class="data row9 col2" >84.3</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow9_col3" class="data row9 col3" >84.6</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow10_col0" class="data row10 col0" >81.0</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow10_col1" class="data row10 col1" >80.6</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow10_col2" class="data row10 col2" >80.9</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow10_col3" class="data row10 col3" >80.4</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow11_col0" class="data row11 col0" >84.1</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow11_col1" class="data row11 col1" >83.4</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow11_col2" class="data row11 col2" >84.4</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow11_col3" class="data row11 col3" >82.8</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow12_col0" class="data row12 col0" >83.7</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow12_col1" class="data row12 col1" >84.3</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow12_col2" class="data row12 col2" >83.6</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow12_col3" class="data row12 col3" >83.8</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow13_col0" class="data row13 col0" >83.9</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow13_col1" class="data row13 col1" >84.0</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow13_col2" class="data row13 col2" >83.8</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow13_col3" class="data row13 col3" >84.3</td> 
    </tr>    <tr> 
        <th id="T_63818512_6abb_11e8_81d2_8c859026d06clevel0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow14_col0" class="data row14 col0" >83.8</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow14_col1" class="data row14 col1" >83.8</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow14_col2" class="data row14 col2" >84.2</td> 
        <td id="T_63818512_6abb_11e8_81d2_8c859026d06crow14_col3" class="data row14 col3" >84.1</td> 
    </tr></tbody> 
</table> 



## SCHOOLS BY SPENDING 


```python
# School_df= District_df
# School_df["Spending Ranges (per student)"] = pd.cut(per_student_bdgt, 
#                                                     bins=spending_bins, labels=group_names)

categories = [0, 585, 615, 645, 675]

group_name = ['< $585', "$585 - 615", "$615 - 645", " $645-675"]

merged_df['spending_bins'] = pd.cut(merged_df['budget']/merged_df['size'], categories, labels = group_name)

#group by spending
by_spending = merged_df.groupby('spending_bins')

#calculations
avg_math = by_spending['math_score'].mean()
avg_read = by_spending['reading_score'].mean()
pass_math = merged_df[merged_df['math_score'] >= 70].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()
pass_read = merged_df[merged_df['reading_score'] >= 70].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()
overall= (pass_math + pass_read)/2
            
# Score by spending data frame            
scores_by_spend = pd.DataFrame({
    "Average Math Score": round(avg_math, 1),
    "Average Reading Score": round (avg_read, 1),
    '% Passing Math': round(pass_math,2),
    '% Passing Reading': round(pass_read,2),
    "Overall Passing Rate": round(overall, 2)
            
})
            
#Organizing columns
scores_by_spend = scores_by_spend[[
    "Average Math Score",
    "Average Reading Score",
    '% Passing Math',
    '% Passing Reading',
    "Overall Passing Rate"
]]

scores_by_spend.index.name = "Spending Ranges (Per Student)"
scores_by_spend = scores_by_spend.reindex(group_name)

#style 
scores_by_spend.style.format({'Average Math Score': '{:.1f}', 
                              'Average Reading Score': '{:.1f}', 
                              '% Passing Math': '{:.0%}', 
                              '% Passing Reading':'{:.0%}', 
                              'Overall Passing Rate': '{:.0%}'})
 




```




<style  type="text/css" >
</style>  
<table id="T_63917af8_6abb_11e8_bb41_8c859026d06c" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Spending Ranges (Per Student)</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_63917af8_6abb_11e8_bb41_8c859026d06clevel0_row0" class="row_heading level0 row0" >< $585</th> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow0_col0" class="data row0 col0" >83.4</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow0_col1" class="data row0 col1" >84.0</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow0_col2" class="data row0 col2" >94%</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow0_col3" class="data row0 col3" >97%</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow0_col4" class="data row0 col4" >95%</td> 
    </tr>    <tr> 
        <th id="T_63917af8_6abb_11e8_bb41_8c859026d06clevel0_row1" class="row_heading level0 row1" >$585 - 615</th> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow1_col0" class="data row1 col0" >83.5</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow1_col1" class="data row1 col1" >83.8</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow1_col2" class="data row1 col2" >94%</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow1_col3" class="data row1 col3" >96%</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow1_col4" class="data row1 col4" >95%</td> 
    </tr>    <tr> 
        <th id="T_63917af8_6abb_11e8_bb41_8c859026d06clevel0_row2" class="row_heading level0 row2" >$615 - 645</th> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow2_col0" class="data row2 col0" >78.1</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow2_col1" class="data row2 col1" >81.4</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow2_col2" class="data row2 col2" >71%</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow2_col3" class="data row2 col3" >84%</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow2_col4" class="data row2 col4" >78%</td> 
    </tr>    <tr> 
        <th id="T_63917af8_6abb_11e8_bb41_8c859026d06clevel0_row3" class="row_heading level0 row3" > $645-675</th> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow3_col0" class="data row3 col0" >77.0</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow3_col1" class="data row3 col1" >81.0</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow3_col2" class="data row3 col2" >66%</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow3_col3" class="data row3 col3" >81%</td> 
        <td id="T_63917af8_6abb_11e8_bb41_8c859026d06crow3_col4" class="data row3 col4" >74%</td> 
    </tr></tbody> 
</table> 



## SCORES BY SCHOOLS SIZE


```python

# create size bins
bins = [0, 1000, 2000, 5000]
group_name = ["Small (<1000)", "Medium (1000-2000)" , "Large (>2000)"]
merged_df['size_bins'] = pd.cut(merged_df['size'], bins, labels = group_name)

#group by spending
by_size = merged_df.groupby('size_bins')

#calculations 
avg_math = by_size['math_score'].mean()
avg_read = by_size['math_score'].mean()
pass_math = merged_df[merged_df['math_score'] >= 70].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()
pass_read = merged_df[merged_df['reading_score'] >= 70].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()
overall = (pass_math + pass_read)/2

            
# df build            
scores_by_size = pd.DataFrame({
    "Average Math Score": avg_math,
    "Average Reading Score": avg_read,
    '% Passing Math': pass_math,
    '% Passing Reading': pass_read,
    "Overall Passing Rate": overall
            
})
            
#reorder columns
scores_by_size = scores_by_size[[
    "Average Math Score",
    "Average Reading Score",
    '% Passing Math',
    '% Passing Reading',
    "Overall Passing Rate"
]]

scores_by_size.index.name = "Total Students"
scores_by_size = scores_by_size.reindex(group_name)

#style
scores_by_size.style.format({'Average Math Score': '{:.1f}', 
                              'Average Reading Score': '{:.1f}', 
                              '% Passing Math': '{:.1%}', 
                              '% Passing Reading':'{:.1%}', 
                              'Overall Passing Rate': '{:.1%}'})
```




<style  type="text/css" >
</style>  
<table id="T_639ab17a_6abb_11e8_b3c4_8c859026d06c" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Total Students</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_639ab17a_6abb_11e8_b3c4_8c859026d06clevel0_row0" class="row_heading level0 row0" >Small (<1000)</th> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow0_col0" class="data row0 col0" >83.8</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow0_col1" class="data row0 col1" >83.8</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow0_col2" class="data row0 col2" >94.0%</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow0_col3" class="data row0 col3" >96.0%</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow0_col4" class="data row0 col4" >95.0%</td> 
    </tr>    <tr> 
        <th id="T_639ab17a_6abb_11e8_b3c4_8c859026d06clevel0_row1" class="row_heading level0 row1" >Medium (1000-2000)</th> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow1_col0" class="data row1 col0" >83.4</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow1_col1" class="data row1 col1" >83.4</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow1_col2" class="data row1 col2" >93.6%</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow1_col3" class="data row1 col3" >96.8%</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow1_col4" class="data row1 col4" >95.2%</td> 
    </tr>    <tr> 
        <th id="T_639ab17a_6abb_11e8_b3c4_8c859026d06clevel0_row2" class="row_heading level0 row2" >Large (>2000)</th> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow2_col0" class="data row2 col0" >77.5</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow2_col1" class="data row2 col1" >77.5</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow2_col2" class="data row2 col2" >68.7%</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow2_col3" class="data row2 col3" >82.1%</td> 
        <td id="T_639ab17a_6abb_11e8_b3c4_8c859026d06crow2_col4" class="data row2 col4" >75.4%</td> 
    </tr></tbody> 
</table> 



## SCORES BY SCHOOL TYPE 


```python
# group by type of school
by_type = merged_df.groupby("type")

#calculations 
avg_math = by_type['math_score'].mean()
avg_read = by_type['math_score'].mean()
pass_math = merged_df[merged_df['math_score'] >= 70].groupby('type')['Student ID'].count()/by_type['Student ID'].count()
pass_read = merged_df[merged_df['reading_score'] >= 70].groupby('type')['Student ID'].count()/by_type['Student ID'].count()
overall= (pass_math + pass_read)/2
# DataFrame            
scores_by_type = pd.DataFrame({
    "Average Math Score": avg_math,
    "Average Reading Score": avg_read,
    '% Passing Math': pass_math,
    '% Passing Reading': pass_read,
    "Overall Passing Rate": overall})
    
#reorder columns
scores_by_type = scores_by_type[[
    "Average Math Score",
    "Average Reading Score",
    '% Passing Math',
    '% Passing Reading',
    "Overall Passing Rate"
]]
scores_by_type.index.name = "Type of School"


#style 
scores_by_type.style.format({'Average Math Score': '{:.1f}', 
                              'Average Reading Score': '{:.1f}', 
                              '% Passing Math': '{:.1%}', 
                              '% Passing Reading':'{:.1%}', 
                              'Overall Passing Rate': '{:.1%}'})
```




<style  type="text/css" >
</style>  
<table id="T_63a1acf4_6abb_11e8_99ec_8c859026d06c" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Type of School</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_63a1acf4_6abb_11e8_99ec_8c859026d06clevel0_row0" class="row_heading level0 row0" >Charter</th> 
        <td id="T_63a1acf4_6abb_11e8_99ec_8c859026d06crow0_col0" class="data row0 col0" >83.4</td> 
        <td id="T_63a1acf4_6abb_11e8_99ec_8c859026d06crow0_col1" class="data row0 col1" >83.4</td> 
        <td id="T_63a1acf4_6abb_11e8_99ec_8c859026d06crow0_col2" class="data row0 col2" >93.7%</td> 
        <td id="T_63a1acf4_6abb_11e8_99ec_8c859026d06crow0_col3" class="data row0 col3" >96.6%</td> 
        <td id="T_63a1acf4_6abb_11e8_99ec_8c859026d06crow0_col4" class="data row0 col4" >95.2%</td> 
    </tr>    <tr> 
        <th id="T_63a1acf4_6abb_11e8_99ec_8c859026d06clevel0_row1" class="row_heading level0 row1" >District</th> 
        <td id="T_63a1acf4_6abb_11e8_99ec_8c859026d06crow1_col0" class="data row1 col0" >77.0</td> 
        <td id="T_63a1acf4_6abb_11e8_99ec_8c859026d06crow1_col1" class="data row1 col1" >77.0</td> 
        <td id="T_63a1acf4_6abb_11e8_99ec_8c859026d06crow1_col2" class="data row1 col2" >66.5%</td> 
        <td id="T_63a1acf4_6abb_11e8_99ec_8c859026d06crow1_col3" class="data row1 col3" >80.9%</td> 
        <td id="T_63a1acf4_6abb_11e8_99ec_8c859026d06crow1_col4" class="data row1 col4" >73.7%</td> 
    </tr></tbody> 
</table> 


