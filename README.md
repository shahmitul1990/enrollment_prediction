# enrollment_prediction

Here is a complete, polished, explanation of how I predicted total student enrollment.
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


### Dataset and features (Variables & Structure)

I collected and cleaned historical enrollment and operational data over several terms/semesters.

Key features in the dataset included:
Student & Application Data

num_inquiries – Number of students who enquired

num_applications – Total applications received

conversion_rate – Application → Enrollment %

program – Program type (MBA, Data Science, Coding, etc.)

mode_of_learning – Online / Offline

location – For offline programs (Mumbai, Bangalore, Delhi, etc.)

Course & Operational Data

tuition_fees – Fee structure

scholarship_offers – Discounts offered

batch_start_date – Month/season factor

class_capacity – Seats available

marketing_spend – Campaign investment

Engagement Data

demo_sessions_attended

followup_calls

time_to_enroll (days between application & payment)

Target Variable

total_enrollment (number of students who finally enrolled)

The dataset had around 8–12 features depending on semester.
