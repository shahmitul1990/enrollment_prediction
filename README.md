# enrollment_prediction

Here is a complete explanation of how I predicted total student enrollment.
<br>It includes:

* [Project background](#project-background)
* [Dataset and features](#dataset-and-features)
* [Models tried](#models-tried)
* [Final model selection](final-model-selection)
* [How accuracy reached 95%](how-accuracy-reached-95%)
* [Impact on the academic institution](impact-on-the-academic-institution)


### Project background
##### Context & Business Problem

The institution runs academic programs both online and offline across a few campuses. One of the biggest operational challenges was:
<br> **Predicting how many students will enroll next term so that faculty hiring, classroom allocation, and study material planning could be optimized.**

They used to overestimate or underestimate demand, leading to:

* Hiring too many or too few faculty
* Overcrowded or underutilized classrooms
* Incorrect budgeting

So the management wanted a predictive enrollment model with high accuracy.


### Dataset and features

I collected and cleaned historical enrollment and operational data over several terms/semesters.

***Key features in the dataset included:***

***Student & Application Data***

* num_inquiries – Number of students who enquired
* num_applications – Total applications received
* conversion_rate – Application → Enrollment %
* program – Program type (MBA, Data Science, Coding, etc.)
* mode_of_learning – Online / Offline
* location – For offline programs (Mumbai, Bangalore, Delhi, etc.)

***Course & Operational Data***

* tuition_fees – Fee structure
* scholarship_offers – Discounts offered
* batch_start_date – Month/season factor
* class_capacity – Seats available
* marketing_spend – Campaign investment

***Engagement Data***

* demo_sessions_attended
* followup_calls
* time_to_enroll (days between application & payment)

***Target Variable***

* total_enrollment (number of students who finally enrolled)

The dataset had around ***8–12*** features depending on semester.

Below is a clean, realistic example of how dataset actually could look for predicting student enrollment for an academic institution that runs online + offline programs.

I’m showing:

* A ***sample dataset (5–6 rows)***
* ***Original raw variables***
* ***Feature-engineered variables you created***
* How each feature was derived

#### Sample Dataset (Before Feature Engineering)

*(5 rows, simplified for demonstration)*

| batch_id | program         | mode    | location  | inquiries | applications | enrollments | tuition_fee | marketing_spend | batch_start_date |
|:--------:|:---------------:|:-------:|:---------:|:---------:|:------------:|:-----------:|:-----------:|:----------------:|:-----------------:|
| B001     | Data Science     | Online  | NA        | 820       | 250          | 180         | 65000       | 320000           | 2023-01-15        |
| B002     | MBA              | Offline | Mumbai    | 540       | 180          | 120         | 90000       | 450000           | 2023-03-10        |
| B003     | Coding Bootcamp  | Online  | NA        | 700       | 210          | 150         | 40000       | 150000           | 2023-07-01        |
| B004     | Data Science     | Offline | Bangalore | 480       | 150          | 110         | 68000       | 300000           | 2023-09-05        |
| B005     | MBA              | Online  | NA        | 620       | 200          | 160         | 85000       | 380000           | 2023-11-20        |

***Target Variable***:
enrollments
<br>This is what out model predicted.



