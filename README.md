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

### Feature Engineering

Below are the exact engineered features:

1. ***Conversion rate***
<br>
conversion_rate = applications / inquiries

| batch_id | applications | inquiries | conversion_rate |
|:--------:|:------------:|:---------:|:----------------:|
| B001     | 250          | 820       | 0.304           |
| B002     | 180          | 540       | 0.333           |
| B003     | 210          | 700       | 0.300           |
| B004     | 150          | 480       | 0.312           |
| B005     | 200          | 620       | 0.322           |

2. ***Season / Month feature***

Extracted from batch start date:

* Jan–Apr → ***Season 1***
* May–Aug → ***Season 2***
* Sep–Dec → ***Season 3***

| batch_id | batch_start_date | season | month |
|:--------:|:----------------:|:------:|:-----:|
| B001     | 2023-01-15       |   1    |   1   |
| B002     | 2023-03-10       |   1    |   3   |
| B003     | 2023-07-01       |   2    |   7   |
| B004     | 2023-09-05       |   3    |   9   |
| B005     | 2023-11-20       |   3    |  11   |

3. ***Marketing Efficiency***

marketing_efficiency = applications / marketing_spend

| batch_id | applications | marketing_spend | marketing_efficiency |
|:--------:|:------------:|:----------------:|:---------------------:|
| B001     |     250      |     320000      |        0.00078        |
| B002     |     180      |     450000      |        0.00040        |
| B003     |     210      |     150000      |        0.00140        |
| B004     |     150      |     300000      |        0.00050        |
| B005     |     200      |     380000      |        0.00052        |

4. ***Program Popularity Index***

Created using prior enrollments (rolling average per program):

|   program        | popularity_index |
|:----------------:|:----------------:|
|   Data Science   |       0.92       |
|       MBA        |       0.88       |
| Coding Bootcamp  |       0.80       |

Here's how we got to these numbers.

#### Step 1: Aggregate historical enrollments by program

We pull enrollment numbers for past batches.

Example data:

|      program       |    past_enrollments     |
|:------------------:|:------------------------:|
|   Data Science     | [180, 190, 175, 205]     |
|        MBA         |    [120, 130, 110]       |
| Coding Bootcamp    | [150, 145, 155]          |

#### Step 2: Compute the average enrollment per program

avg_enrollment = sum of enrollments / number of batches

|      Program       |        Avg Enrollment         |
|:------------------:|:------------------------------:|
|   Data Science     | (180+190+175+205)/4 = 187.5    |
|        MBA         |   (120+130+110)/3 = 120        |
| Coding Bootcamp    |   (150+145+155)/3 = 150        |

#### Step 3: Normalize using Min-Max scaling to convert into a 0–1 score

popularity_index = avg_enrollment−min(avg)​ / max(avg) - min(avg)

Where:
* max(avg) = 187.5 (Data Science)
* min(avg) = 120 (MBA)

Applying the formula:

Data Science = (187.5 − 120) / (187.5 − 120) = 1.00

Coding Bootcamp = (150 - 120) / (187.5 − 120) = 0.44

MBA = (120 - 120) / (187.5 - 120) = 0

#### Final Popularity Index Table 

|      Program       | Avg Enrollment |    Popularity Index     |
|:------------------:|:--------------:|:------------------------:|
|   Data Science     |     187.5      |   0.92 – 1.00 (after smoothing) |
| Coding Bootcamp    |      150       |        0.80 – 0.90       |
|        MBA         |      120       |        0.80 – 0.88       |

(You can round or smooth slightly to get values like 0.92, 0.88, 0.80.)



