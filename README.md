# Pandas-Challenge

#The following code generated based on teachings from Monash Lesson Plans, getting mean, sum, unique, value_counts method in the series
##start code

#calculate total number of schools
total_number_schools = len(school_data_complete["school_name"].unique())

#calculate total number of students
total_number_students = school_data["size"].sum()

#calculate total budget
total_budget = school_data["budget"].sum()

#calculate average maths score
average_math = student_data["maths_score"].mean()

#calculate average reading score
average_reading = student_data["reading_score"].mean()

##end code

#The following code generated based on teachings from Monash Lesson Plans, using .loc in filtering data
##code

#calculate percentage of students with a passing math score (50 or greater)
maths_score_50 = student_data.loc[student_data["maths_score"]>=50]
maths_score = len(maths_score_50) / total_number_students * 100

#calculate percentage of students with a passing reading score (50 or greater)
reading_score_50 = student_data.loc[student_data["reading_score"]>=50]
reading_score = len(reading_score_50) / total_number_students * 100

#percentage of students who passed maths and reading (% Overall Passing)
maths_and_reading_overall = student_data.loc[(student_data["maths_score"]>=50)
                                    & (student_data["reading_score"]>=50)]
maths_and_reading = len(maths_and_reading_overall) / total_number_students * 100

##end code

#The following code generated based on teachings from Monash Lesson Plans, placing all calculated into a summary DataFrame
##code
#create a dataframe to hold above results
#Optional: give the displayed data cleaner formatting
area_summary = pd.DataFrame({"Total Schools": [total_number_schools],
                                        "Total Students": [total_number_students],
                                        "Total Budget": [total_budget],
                                        "Average Maths Score": [average_math],
                                        "Average Reading Score": [average_reading],
                                        "%Passing Maths": [maths_score],
                                        "%Passing Reading": [reading_score],
                                        "%Overall Passing": [maths_and_reading]})

##end code

#The following code generated based on teachings from Monash Lesson Plans, formatting variables
##code

area_summary["Total Students"] = area_summary["Total Students"].map("{:,}".format)
area_summary["Total Budget"] = area_summary["Total Budget"].map("${:,.2f}".format)

#end code

#The following code generated based on teachings from Monash Lesson Plans, setting index, using groupby , merging files and using mean and value_counts method. Generated code and debugging with the guidance of AskBCs LA Peter and Sunshine. 

##code
#school type
school_types = school_data.set_index(["school_name"])["type"] 
school_types

#total students - school_data file (merged file)
merged_total_students = school_data_complete["school_name"].value_counts()
merged_total_students

#total school budget - school_data file (merged file)
merged_total_budget = school_data_complete.groupby(["school_name"])["budget"].mean()
merged_total_budget

#Per Student budget
merged_student_budget = merged_total_budget / merged_total_students
merged_student_budget

#Average Maths Score
merged_average_math = school_data_complete.groupby(["school_name"])["maths_score"].mean()
merged_average_math

#Average Reading Score
merged_average_reading = school_data_complete.groupby(["school_name"])["reading_score"].mean()
merged_average_reading

##end code

#The following code generated based on teachings from Monash Lesson Plans, using .loc in filtering data andf using groupby function. Code generated with AskBCs LA Sean.

##code

#% Passing Maths
students_passing_math = school_data_complete.loc[school_data_complete["maths_score"] >= 50]
merged_passing_math = students_passing_math.groupby("school_name").size() / school_data_complete.groupby("school_name").size()*100
merged_passing_math

#% Passing Reading
students_passing_reading = school_data_complete.loc[school_data_complete["reading_score"] >= 50]
merged_passing_reading = students_passing_reading.groupby("school_name").size() / school_data_complete.groupby("school_name").size()*100
merged_passing_reading

#% Overall Passing 
students_passing_overall = school_data_complete.loc[(school_data_complete["maths_score"]>=50)
                                                    & (school_data_complete["reading_score"]>=50)]
merged_overall_passing = students_passing_overall.groupby("school_name").size() / school_data_complete.groupby("school_name").size()*100
merged_overall_passing

##end code

#The following code generated based on teachings from Monash Lesson Plans, placing all calculated into a summary DataFrame
##code

#Create a dataframe to hold the above results
per_school_summary = pd.DataFrame({"School Type": school_types,
                                   "Total Students": merged_total_students,
                                   "Total School Budget": merged_total_budget,
                                   "Per Student Budget": merged_student_budget,
                                   "Average Maths Score": merged_average_math,
                                  "Average Reading Score": merged_average_reading,
                                  "% Passing Maths":merged_passing_math,
                                  "% Passing Reading": merged_passing_reading,
                                  "% Overall Passing": merged_overall_passing})

##end code

#The following code generated based on teachings from Monash Lesson Plans, formatting variables
##code

per_school_summary["Total School Budget"] = per_school_summary["Total School Budget"].map("${:,.2f}".format)
per_school_summary["Per Student Budget"] = per_school_summary["Per Student Budget"].map("${:,.2f}".format)

##end code

#The following code generated based on teachings from Monash Lesson Plans, sorting values
##code

#Sort and display the top five performing schools by % overall passing
top_schools = per_school_summary.sort_values("% Overall Passing", ascending=False)
top_schools.head()

#Sort and display the five worst-performing schools by % overall passing
bottom_schools = per_school_summary.sort_values("% Overall Passing", ascending=True)
bottom_schools.head()

##end code

#The following code generated based on teachings from Monash Lesson Plans, using .loc in filtering data andf using groupby function
##code

#Use .loc for conditional statements
year_9 = student_data.loc[student_data["year"] == 9, :]
year_10 = student_data.loc[student_data["year"] == 10, :]
year_11 = student_data.loc[student_data["year"] == 11, :]
year_12 = student_data.loc[student_data["year"] == 12, :]

#groupby school_name
year_9_average = year_9.groupby(["school_name"])["maths_score"].mean()
year_10_average = year_10.groupby(["school_name"])["maths_score"].mean()
year_11_average = year_11.groupby(["school_name"])["maths_score"].mean()
year_12_average = year_12.groupby(["school_name"])["maths_score"].mean()

##end code

#The following code generated based on teachings from Monash Lesson Plans, placing all calculated into a summary DataFrame
##code

#Combine the series into a dataframe
#Optional: give the displayed data cleaner formatting
maths_scores_by_year = pd.DataFrame ({"Year 9": year_9_average,
                                "Year 10": year_10_average,
                                "Year 11": year_11_average,
                                "Year 12": year_12_average})
##end code

#The following code provided by AskbcS LA Catherine, renaming index
##code

maths_scores_by_year.index.names = ['']

##end code

#The following code generated based on teachings from Monash Lesson Plans, using .loc in filtering data andf using groupby function
##code

#Use .loc for conditional statements
year_9 = student_data.loc[student_data["year"] == 9, ]
year_10 = student_data.loc[student_data["year"] == 10, ]
year_11 = student_data.loc[student_data["year"] == 11, ]
year_12 = student_data.loc[student_data["year"] == 12, ]

#groupby school_name
year_9_average = year_9.groupby(["school_name"])["reading_score"].mean()
year_10_average = year_10.groupby(["school_name"])["reading_score"].mean()
year_11_average = year_11.groupby(["school_name"])["reading_score"].mean()
year_12_average = year_12.groupby(["school_name"])["reading_score"].mean()

##end code

#The following code generated based on teachings from Monash Lesson Plans, placing all calculated into a summary DataFrame
##code

#Combine the series into a dataframe
#Optional: give the displayed data cleaner formatting
reading_scores_by_year = pd.DataFrame ({"Year 9": year_9_average,
                                "Year 10": year_10_average,
                                "Year 11": year_11_average,
                                "Year 12": year_12_average})

#The following code provided by AskbcS LA Catherine, renaming index
##code

reading_scores_by_year.index.names = ['']

##end code

#The following code provided in the challenge and generated based on teachings from Monash Lesson Plans, binning
##code

#Provided code
spending_bins = [0, 585, 630, 645, 680]
labels = ["<$585", "$585-630", "$630-645", "$645-680"]

#Use pd.cut to categorise spending based on the bins.
school_spending_df["Spending Ranges (Per Student)"] = pd.cut (merged_student_budget, spending_bins, labels=labels)
school_spending_df

##end code

#The following code generated based on teachings from Monash Lesson Plans, using groupby function and mean method
##code

#Use the following code to then calculate mean scores per spending range
#provided code
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Maths Score"].mean()
spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Reading Score"].mean()
spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Maths"].mean()
spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Reading"].mean()
overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Overall Passing"].mean()

##end code

#The following code generated based on teachings from Monash Lesson Plans, placing all calculated into a summary DataFrame
##code

#Create a DataFrame 
spending_summary = pd.DataFrame({"Average Maths Score": round (spending_math_scores, 2),
                                 "Average Reading Score": round (spending_reading_scores, 2),
                                 "% Passing Maths": round (spending_passing_math, 2),
                                 "% Passing Reading": round (spending_passing_reading, 2),
                                 "% Overall Passing": round (overall_passing_spending, 2) })
##end code

#The following code provided in the challenge and generated based on teachings from Monash Lesson Plans, binning
##code

#Provided code
size_bins = [0, 1000, 2000, 5000]
labels = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]

#Duplicate per_school_summary data frame for reference
school_size_df = per_school_summary.copy()

#Use pd.cut on the "Total Students" column of the per_school_summary DataFrame
school_size_df["School Size"] = pd.cut(school_spending_df["Total Students"], size_bins, labels=labels)
school_size_df

##end code

#The following code generated based on teachings from Monash Lesson Plans, using groupby function and mean method
##code

#Code to then calculate mean scores per school size
size_math_scores = school_size_df.groupby(["School Size"])["Average Maths Score"].mean()
size_reading_scores = school_size_df.groupby(["School Size"])["Average Reading Score"].mean()
size_passing_math = school_size_df.groupby(["School Size"])["% Passing Maths"].mean()
size_passing_reading = school_size_df.groupby(["School Size"])["% Passing Reading"].mean()
overall_passing_size = school_size_df.groupby(["School Size"])["% Overall Passing"].mean()

##end code

#The following code generated based on teachings from Monash Lesson Plans, placing all calculated into a summary DataFrame
##code

#Create a DataFrame called size_summary that breaks down school performance based on school size (small, medium, or large)
size_summary = pd.DataFrame({"Average Maths Score": size_math_scores,
                             "Average Reading Score": size_reading_scores,
                            "% Passing Maths": size_passing_math,
                            "% Passing Reading": size_passing_reading,
                            "% Overall Passing": overall_passing_size})
size_summary

##end code

#The following code generated based on guidance by AskBCs LA Sean.
##code

#Duplicate per_school_summary data frame for reference
school_type_df = per_school_summary.copy()

##end code

#The following code generated based on teachings from Monash Lesson Plans, using groupby function and mean method
##code

#Code to then calculate mean scores per school type
type_math_scores = school_type_df.groupby(["School Type"])["Average Maths Score"].mean()
type_reading_scores = school_type_df.groupby(["School Type"])["Average Reading Score"].mean()
type_passing_math = school_type_df.groupby(["School Type"])["% Passing Maths"].mean()
type_passing_reading = school_type_df.groupby(["School Type"])["% Passing Reading"].mean()
overall_passing_type = school_type_df.groupby(["School Type"])["% Overall Passing"].mean()

##end code

#The following code generated based on teachings from Monash Lesson Plans, placing all calculated into a summary DataFrame
##code

#Create a DataFrame called type_summary to show school performance based on the "School Type"
type_summary = pd.DataFrame({"Average Maths Score": type_math_scores,
                             "Average Reading Score": type_reading_scores,
                            "% Passing Math": type_passing_math,
                            "% Passing Reading": type_passing_reading,
                            "% Overall Passing": overall_passing_type})
type_summary

##end code
