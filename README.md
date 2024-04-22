![CC Graphics 2024)SurveysonQualtrics](https://github.com/csae-coders-corner/Personalised-surveys-on-Qualtrics/assets/148211163/157c441e-6231-4c78-9bc5-6d21a3ef6056)

# Personalised-surveys-on-Qualtrics
[Qualtrics](https://www.qualtrics.com/uk/) (like SurveyCTO) is a software that allows researchers to design questionnaires and record responses. It can be accessed via https://www.qualtrics.com/uk/and you can also make a free account. 

Adding questions on Qualtrics is a simple process but, sometimes, surveys must be specific to the respondent who is answering it. For example, the head of a company may be asked specific questions about each of her employees or a teacher may be asked questions about each of her students. How can we go through a roster of employee/student names with the least hassle? How can we personalize the surveys to improve respondent experience? 

This post provides the solution to this problem pointed out by a very helpful user on the [Qualtrics’ Community website](https://www.qualtrics.com/community/discussion/7243/importing-student-information-for-different-teachers). The solution was jointly implemented by [Ronak Jain](https://scholar.harvard.edu/ronakjain/home) and me in our recent work. I will go through the problem and a candidate solution using a simple example. 


## Problem

Let T be the set of all teachers in a group of schools and S_(t ) be the set of students of teacher t ∈ T. Let’s say we are interested in surveying teachers and asking them student specific questions. For example, we want to ask teacher k how hard student s ∈ S_(k )worked last week. We want to do this for all teachers and their students. How should we code this in Qualtrics? 

The first option is to add a separate question for each student in our sample and add a question-specific condition using the “display logic” item. This means that we can first ask the teacher k her name and then for each question about all students, we can add a condition that makes sure that the question “How hard did student s work” is only asked when s ∈ S_(k ). 

If we have S= |∪_(t∈T)▒S_(t )  | students in our sample, this means that we will first copy our question, S times and then for each of these, add a “display logic” condition i.e. ask question for student s for teacher k if the teacher’s name (as entered/selected by her) k. 

Apart from the sheer number of times each question will have to be copied, additional problems arise if teachers accidentally misspell their names or we do not have perfectly accurate names. The display logic conditions will not work. What can we do? 

## Solution

### Import a Contact List

Qualtrics allows users to create contact lists i.e. a list of respondent email IDs, names, and other characteristics that can be imported into the Qualtrics library as an excel file.  These are primarily used to distribute surveys to respondents, but as we will see, they can also be used to create personalised surveys. The contact list for our example can look like this:

![qualtrics 1](https://github.com/csae-coders-corner/Personalised-surveys-on-Qualtrics/assets/148211163/80f2c1cf-e6d5-47dc-8706-f34f250a3604)

Notice a few things:

1.	While I have named the variables here, Qualtrics will interpret the first row as a contact. The variables are only named here for the sake of discussion in this post.  
2.	The variables Email, FirstName, and LastName must be the first three variables followed by any demographics of our choice which in this case, include school name, class, section, and the names of students. 
3.	Student names are added as columns. There will be as many columns as the maximum number of students taught by a teacher. In our example, this number is 4 even though teacher2 and teacher3 have fewer students. 

We can now import this contact list into the Qualtrics library. 

Once the contact list is imported, we import it into our survey as “embedded data”. We can do this via the survey flow option. The steps are highlighted in the pictures below- 

#### Step 1a: Click on the option “add a new element here”

![qualtrics 2](https://github.com/csae-coders-corner/Personalised-surveys-on-Qualtrics/assets/148211163/86946b64-57bc-4dd1-97b7-2e2ee6d3d604)

#### Step 1b: Choose the embedded data option

![qualtrics 3](https://github.com/csae-coders-corner/Personalised-surveys-on-Qualtrics/assets/148211163/99b38e9e-88b2-47f8-9488-51284c2f4a33)

#### Step 2:  Click on “Add from contacts” and select the contact file from the library

![qualtrics 4](https://github.com/csae-coders-corner/Personalised-surveys-on-Qualtrics/assets/148211163/11130fad-7c3b-4241-a597-a1bbf74d285e)

#### Step 3: Move the embedded data to the beginning of the survey so that the variables are stored in the beginning of the survey and can be used thereafter. 

Once this is done, the variables “School”, “Class”, …”Student4” are created as embedded variables. These variables can be referenced throughout the survey. 

### Write the questions

Now, when we have to ask, “How hard did the student ABC work during last week”? we can reference the student name variables instead and ask four questions- 

1.	How hard did ${e://Field/Student1} work during last week? 
2.	How hard did ${e://Field/Student2} work during last week? 
3.	How hard did ${e://Field/Student3} work during last week? 
4.	How hard did ${e://Field/Student4} work during last week? 

Now, remember that some teachers teach fewer students than others. 

So, we should add as many questions as the maximum number of students in any class (which is 4 here) and then add a display logic that only asks the question if the variable isn’t empty. 

This can be done in the following manner: 

![qualtrics 5](https://github.com/csae-coders-corner/Personalised-surveys-on-Qualtrics/assets/148211163/abd49456-c9f4-4530-aa1a-d9cfe5e082aa)

Instead of writing the questions as many times as the total number of students in the sample and adding a unique condition for each question (prone to typing errors on behalf of the respondent), we now write the question only as many times as the total number of students in the class with the highest size and add a non-missing display logic to all of them. 

### Create Personalized Links

Once all questions are written in this manner, we can also personalize this survey further. 

Qualtrics automatically stores the variable FirstName as ${e://Field/RecipientFirstName} when we import the contact list into the survey as embedded data. The first block of the survey can now say something like “Welcome ${e://Field/RecipientFirstName}!” so that whoever opens the link sees their name on the screen. We can also add backcheck questions such as “Do you teach in class ${e://Field/Class}  in school ${e://Field/School} ?”  

Note that personalising the survey like this and adding questions using rows of the contact list implies that the survey link will have to be unique for each respondent. 

The Distributions tab (see below) allows us to generate respondent-specific links. 

![qualtrics 7](https://github.com/csae-coders-corner/Personalised-surveys-on-Qualtrics/assets/148211163/1067d9ea-3988-48a6-9b58-2ab68d6d7de6)

We can then write to all the respondents (and schedule reminders) using their emails from the contact list. These emails will contain their unique personalised links. This can be done in just a few simple clicks! 

**Vatsal Khandelwal, DPhil candidate in Economics, Linacre College
26 January 2021**
