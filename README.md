## Analysis of UC Davis Instructional System --- a case study of 2018 winter quarter

### Group members  

Yuxin Lu  Dandi Peng Yuyang Du Jiayi Bian
![6](https://github.com/yxlu1cindy/picture/blob/master/Capture.PNG)

### Reasons why we choose this topic

Every quarter we have to register for some courses. Therefore, we are eager to know more about course details by analyzing this system. We also want to find more information about textbooks since textbooks are required for almost every course.  

We also want to combine this system with other websites to acquire some information about professor rating, which will help us make a better choice when we register for courses. 

### Datasets we use
- [Course detail](https://registrar.ucdavis.edu/courses/search/index.cfm)
- [Textbooks](http://ucdavisstores.com/SelectTermDept)
- [Rate my professor](http://www.ratemyprofessors.com/mobile/school_search)
- [CourseCRN](http://ucdavisstores.com/SelectTermDept)
- [department](https://registrar.ucdavis.edu/records/transcripts/subject-codes.cfm)
- [Buildings](https://maps.googleapis.com/maps/api/geocode/json) -- Google Map API

### Data extraction and Storage  

The scraping code is in [Scrapt_Code file](https://github.com/yxlu1cindy/Python-Project/tree/master/Scrape_Code).  
  
- Course detail
  1. File Name: CourseSchedule.ipynb
  2. Scraping Method: Web Scraping by requests and parsing the response by BeautifulSoup
  3. Data Range: 2018Winter Quarter; including all departments that open courses this quarter except the Law School
  4. Data Size: 13814 rows, 12 columns (before data cleaning)
  5. Variables (without the features that we do not use in our analysis):  
  (a) Available Seats: the remaining seats of a course;  
  (b) CRN: the unique key of a course;  
  (c) Instructor: the instructor of the course;  
  (d) L_T: time and location of the coures;  
  (e) Maximum Seats: the maximum student numbers a course allows;  
  (f) subjuect: the department the course offered by  
  (g) title: course name

- Textbook
  1. File Name: bookstore.ipynb
  2. Scraping Method: Posting requests with cookies and parsing the response by BeautifulSoup
  3. Data Range: 2018Winter Quarter; including all courses that uses textbooks in this quarter except for courses opened by    the Law School
  4. Data Size : 3165 rows, 12 columns (before data cleaning)
  5. Variables (without the features that we do not use in our analysis):  
  (a) coursename: course name;  
  (b) coursesection: course section;  
  (c) Instructor: the instructor of the course;  
  (d) book_needed: the type of textbooks(Required, Optional or Special Order Only);  
  (e) book_name: the name of the textbook;  
  (f) book_pubtime: the publish time of the textbook
 
 - RateMyProfessor     
   1. File Name: RateMyProfessor-final.ipynb
   2. Scraping Method: Posting requests with cookies and parsing the response by BeautifulSoup   
   3. Data Range: 2018Winter Quarter; including all professors that instruct in this quarter except for those haven't been rated on the website
   4. Data Size : 21099 rows, 15 columns (before data cleaning)
   5. Variables (without the features that we do not use in our analysis):  
   (a) Attendance: Mandatory/Not Mandatory/NA;  
     (b) Class: Class name (Department abbreviation + course number);  
     (c) For Credit: Yes/No/NA;  
     (d) Grade Received: A+ —— F/NA;  
     (e) Level of Difficulty: the difficulty students feel, varies from 1 to 5, the higher, the more difficult;  
     (f) Overall Quality: rating for the course, varies from 1 to 5 in float format, the higher, the better;  
     (g) Professor: the professor's name of this course;  
     (h) Rating Type: rating for the professor, varies from "awesome" to "awful" in 5 levels;  
     (i) Textbook: Textbook used or not -- Yes/No/NA;  
     (j) Would Take Again: would like to take courses of this professor or not -- Yes/No/NA;  
     (k) Comment tags: tags that can best describe the professor;  
     (l) Comment: Specific comment for the professor;  
     (m) date: the date of the rating.
 
 - Department   
    1. Data Range: all the subjects in UC Davis  
    2. Data Size : 400 rows, 3 columns
    3. Variables (without the features that we do not use in our analysis):   
    (a) Subject_Code: the abbreviation of the subject;  
    (b) Subject_Description: the full name of the subject
 
 - Buildings
    1. File Name:  Get_location.ipynb
    2. Scraping Method: Using Goole Map Api to get data and parsing the response by BeautifulSoup   
    3. Data Range: all the classrooms that have courses in 2018Winter Quarter   
    4. Data Size : 465 rows, 4 columns (before data cleaning)
    5. Variables (without the features that we do not use in our analysis):   
    (a) location: name of the classroom;  
    (b) represent: the latitude and the longitude of the classroom

### Data exploration  

After getting all of the data, we dropped duplicated lines, reformatted the dataset and stored them altogether in data.db file.  
The code is in [Data Reformatting.ipynb](https://github.com/yxlu1cindy/Python-Project/blob/master/Data%20Reformatting.ipynb).  
We also explore head, shape, data types of each column and whether each column has missing values for each dataset. We also use describe function to help us get summary statistics of numeric variables.

### Data analysis

**1. What are popular departments?**  

We calculate number of courses in each department and define those who have largest number of courses as popular departments. 
We also draw barplot of number of courses in each department.

![1](https://user-images.githubusercontent.com/35319675/37563387-ba2216d0-2a3c-11e8-91de-fd0befdcaf0f.png)  

From the plot we can see that department of **Chemistry, Physics, Writing Program, Mathematics and Economics** are popular departments.  

We also calculate number of textbooks used in each department and draw the plot.  
From the plot we can see that those popular departments we find before are also departments who use textbooks most frequently.
 

**2. What are popular courses?**  

Here we use three strategies to define popular courses.  
The first one is based on course frequency, the second one is based on maximum seats of courses and the last one is based on both maximum and available seats of courses. We also draw three plots of popular courses.  
From these plots we can conclude that **General Chemistry, General Physics, Calculus, Calculus for Biology Science and Introduction to Biology** are popular courses.  
Moreover, results in these three plots are consistent with each other.


**3. How about course distribution based on weekday?**  

We calculate number of courses on different days and draw the plot.  

![2](https://user-images.githubusercontent.com/35319675/37563513-a1d051e8-2a3f-11e8-97c0-d56b744f413c.png)

From the plot we can see that on **Tuesday**, number of courses is the largest. 


**4. How about course distribution based on time and location?**  

We draw a plot to show number of courses at different times and locations.

<img width="577" alt="6" src="https://user-images.githubusercontent.com/35319675/37569965-00fd0f96-2aa7-11e8-8736-87b03bdcae91.png">
 
Course distribution can also be shown as student distribution. Those places with more studets are places who are holding more classes. Therefore in this part we draw the heatplot to show the student distribution.  

By clicking buttons to change different weekdays, beginning hours and ending hours, we can see student distribution directly. Note that if we choose weekday as 0, the plot shows the overall student distribution during a week.


**5. What is the ratio of number of students to number of professors of each course?**  

We draw a distribution plot of the ratio.  

![4](https://user-images.githubusercontent.com/35319675/37572080-18685f0c-2ac3-11e8-9e66-4325e23afb52.png)

From the distribution plot we can see that for most courses, the ratio is between **0 and 0.025**.  
Here we want to check what kind of depatments tend to have lower ratio.
Therefore, we find departments with lower ratio and draw the barplot.

![12](https://user-images.githubusercontent.com/35319675/37572105-6c363140-2ac3-11e8-9195-4bc3bd42be20.png)

We can find **Physical Education, Mathematics, History, Physics and Political Science** has smallest ratio. Compared it to the plot of popular departments, we can find that there is a **negative relationship** between popular departments and the ratio. That is, more popular the department is, smaller the ratio is. However, among the popular departments, we can also conclude that in similar extent of popularity, departments in arts tend to have a smaller ratio than that in science.

**6. How about overall quality in rate my professor data?**  

Overall quality shows how students are satisfied with the courses. Larger the value is , more satisfied are the students.
We pick up departments with the largest overall quality.

![5](https://user-images.githubusercontent.com/35319675/37563536-31ef06de-2a40-11e8-8d48-d7957709de06.png)

Note : Analysis of above questions can be shown in [Course_analysis_version4.ipynb](https://github.com/yxlu1cindy/Python-Project/blob/master/Course_analysis_version4.ipynb).


**7. Can we build regression models based on rate my professor data?**

We build a multiple linear regression model using forward selection method on a training set (75% of data). The result indecates that the model fit the data well with regression coefficients over 95%, and prediction on the testing set (25% of data) is also very satisfiable.

![7](https://user-images.githubusercontent.com/35319675/37570490-ac48d05a-2aad-11e8-8577-d042000dbc83.png)

The model summary shows that the overall quality of a course can be predicted by the level of difficulty, rating type of professor and course level, while uncorrelated with the fact that whether it is a STEM course. Generally, an easier and lower degree course taught by a highly rating professor achieves a better overall quality in the rate_my_professor data.

The overall quality can also be fitted by a KNN Regression model. We search a model with best number of neighbors (k) that achieves the optimal combination of the prediction error and misclassification rate creteria.

![8](https://user-images.githubusercontent.com/35319675/37570506-cefae48a-2aad-11e8-9ae4-cb2775d02f3b.png)

By testing k from 1 to 20, we find that a KNN Regression model with k = 2 perform best in prediction on the testing set, with about 83% courses predicted right.

**8. Can we draw word cloud to see and figure out the difference among comment tags students selected to describe professors in different RatingType?**

One amazing data we scrapped from "RateMyProfessor" are comment tags students selected and comments they wrote that can best describe the professor. By dividing comment tags with the RatingType of professors into three parts -- "awesome & good", "average", "poor & awful", we got three word clouds below:  

![9](https://user-images.githubusercontent.com/35319675/37570514-e39bb162-2aad-11e8-8972-779ad4f3020f.png)

![10](https://user-images.githubusercontent.com/35319675/37570520-f990afea-2aad-11e8-8a94-00d0c2f73222.png)

![11](https://user-images.githubusercontent.com/35319675/37570525-0f77c5c8-2aae-11e8-87af-85df0cef4f77.png)

Viewing the above three Word Clouds, it is apparent that for the professor with a higher rating, "AMAZING LECTURES", "GIVES GOOD FEEDBACK", "CLEAR GRADING CRITERIA" are most common phrases, also "ACCESSIBLE OUTSIDE" is one of phrases very different from ratings in other levels. For the professor with an average or lower rating, "TOUGH GRADER", "SKIP CLASS? YOU WON'T PASS", "GET READY TO READ", "HOMEWORK TOUGH", "LOTS HOMEWORK", and "LECTURE HEAVY" are the most frequent phrases. As for the difference between 'average' and 'lower' levels, more words related to "Grade" show up at the lower levels, such as "GRADE THINGS", "GRADER TOUGH".  

Note : Analysis of linear regression model, KNN model, and word cloud can be shown in [Linear_Regression_Model-ratemyprofessor-V6.ipynb](https://github.com/yxlu1cindy/Python-Project/blob/master/Linear%20Regression%20Model-ratemyprofessor-V6.ipynb).


### Summary and conclusion
Through exploration and analysis of our datasets, we can conclude that departments such as Physics, Mathematics and Chemistry are what we defined as popular departments. As far as we are concerned, this is because these departments are fundemental and most majors in these departments are STEM, which will be very helpful for students when they seek jobs. Accordingly, what we defined as popular courses are also in these popular departments and these departments are also those who use textbooks most frequently. 

Moreover, we can conclude that these popular departments have smaller ratio of number of students to number of professors. From our point of view this is because these popular departments tend to hire more professors to meet students' need. Therefore, our results got from our datasets are consistent with each other.


Professors, as the core of courses, are the most important part for us to play with statistical and machine learning tools. After scraping more than 20,000 raw data from 'Rating My Professors' website, we complete data processing, exploratory data analysis and apply regression models to it.

According to our data analysis, the overall quality of a course can be modeled by multiple linear regression as well as K-Nearest-Neighbors (KNN) Regression. Both  models fit the data well and produce good predictions.

  1. The multiple linear regression model tells the important factors that decide a course's overall quality: level of difficulty, rating type of professor and course level. Although, obviously, students favor easy lower level courses, professor still play the dominant role in evaluation of a course.

  2. The KNN Regression model with k=2 predict nearly perfectly for courses of quality 1,3 and 5, and make more mistakes when making prediction for courses of quality 2 and 4, so it might be the case that the bad, average and good courses are very alike in their own groups, while the bondaries in between are blurred.

Since the models work well, they can help students predict the courses that have not been rated on the website according to the course difficulty, level and its information of instructors, which may give them valuable suggestions when choosing GE courses.


Another amazing data we collected are comment tags students selected and comments they wrote to best describe professors. Through Natural Language Processing (NLP), we figure out the most common words and phrases students used. In other words, we find out the important parts students care about, which can be summarized as the following:

  1. "AMAZING LECTURES", "GIVES GOOD FEEDBACK", "CLEAR GRADING CRITERIA" are most common characters of professors with a high rating, also "ACCESSIBLE OUTSIDE" is the one deserved to be emphasized.

  2. For average and lower ratings, words related to "GRADE" show a lot, "HEAVY/TOUGH HOWEWORK/EXAM" is a majority as well. This demonstrates that students are concerned about grades, which is really reasonable that a lower grade probably has a negative impact on students' impression in professors/courses.

                 
## Thank you very much!

