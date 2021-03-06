###/surveys/{id}/pages/{id}/questions

>Definition

```
POST https://api.surveymonkey.net/v3/surveys/{survey_id}/pages/{page_id}/questions
```

>Example Request

```shell
curl -i -X POST -H "Authorization:bearer YOUR_ACCESS_TOKEN" https://api.surveymonkey.net/v3/surveys/{survey_id}/pages/{page_id}/questions?api_key=YOUR_API_KEY -d '{"headings": [{"heading": "A question about primates.", "random_assignment": {"percent": 50, "position": 1}},{"heading": "A question about primates phrased slightly differently.", "random_assignment": {"percent": 50, "position": 2},}], "family": "open_ended", "subtype": "single", "position": 1, "sorting": {"type": "textasc","ignore_last": True}, "required": {"text": "This question is required!", "type": "at_least", "amount": "1"},"validation": {"type": "integer","text": "Validation has failed!","min": 20,"max": 30}, "forced_ranking": True, "answers": { #SEE ANSWERS SECTION }}'

```


```python
import requests

s = requests.session()

payload = {
  "headings": [
    {
      "random_assignment": {
        "position": 1,
        "percent": 50
      },
      "heading": "Multiple Choice with Random Assignment v1"
    },
    {
      "random_assignment": {
        "position": 2,
        "percent": 50
      },
      "heading": "Multiple Choice with Random Assignment v2"
    }
  ],
  "family": "single_choice",
  "subtype": "vertical",
  "answers": {
    "choices": [
      {
        "text": "a",
        "visible": true,
        "position": 1
      },
      {
        "text": "b",
        "visible": true,
        "position": 2
      },
      {
        "text": "c",
        "visible": true,
        "position": 3
      }
    ]
  },
  "position": 3
}
url = "https://api.surveymonkey.net/v3/surveys/%s/pages/%s/questions" % (survey_id, page_id, YOUR_API_KEY)
s.post(url, data=payload)

####Response
Same as request, but with two additional fields (id, href)

```

####Available Methods

 * `HEAD`: Checks if resource is available
 * `OPTIONS`: Returns available methods and options
 * `GET`: Returns a list of questions on a survey page
 * `POST`: Creates a new question on a survey page, see [formatting question types](#formatting-question-types)

####Optional Query Strings for GET

Name | Description | Type
------ | ------- | -------
page | Which page of resources to return. Defaults to 1 | Integer
per_page | Number of resources to return per page | Integer

####Question List Resource

Name | Description | Type
------ | ------- | -------
data[\_].id | Question id | String
data[\_].heading | Question heading | String
data[\_].href | Resource API URL | String
data[\_].position | Position of question on page | Integer

####Request Body Arguments for POST

Name | Required |Description | Type
----- | ------ |------ | -----
headings | Yes |Question heading | List
headings[\_].heading | Yes |The title of the question, or empty string if `random_assignment` is defined | String
headings[\_].description | No | If `random_assignment` is defined, and `family` is `presentation_image` this is the title | String
headings[\_].image | No |Image data when question `family` is `presentation_image`| Object or null
headings[\_].image.url | No | URL of image when question `family` is `presentation_image`| String
headings[\_].random_assignment | No |Random assignment data | Object or null
headings[\_].random_assignment.percent | Yes |Percent chance of this random assignment showing up (must sum to 100) | Integer
headings[\_].random_assignment.position | No |Position of the random_assignment in survey creation page | Integer
headings[\_].random\_assignment.variable\_name | No |Internal use name for question tracking, can be `""` | String
position | No (default=last) | Position of question on page | Integer
visible | No default=True) | Whether the question is visible (corresponds with being deleted in the UI) | Boolean
family | Yes | Question type, see [formatting question types](#formatting-question-types) | String
subtype | Yes | Question type's subtype, see [formatting question types](#formatting-question-types)  | String
sorting | No | Sorting details of answers | Object
sorting.type | Yes | Sort answer choices by: "default", "textasc", "textdesc", "resp_count_asc", "resp_count_desc", "random", "flip"| String
sorting.ignore_last| No | Do not sort the last answer option (useful for "none of the above", etc) | Boolean
required | No | Whether an answer is required for this question | Object
required.text | Yes | String to display if a required question is not answered | String
required.type | Yes if question is matrix_single, matrix_ranking, and matrix menu | Required type: "all" , "at_least", "at_most", "exactly", or "range" | String
required.amount | Yes if type is defined | The amount of answers required to be answered. If the required type is range then this is two numbers separated by a comma, as a string (e.g. "1,3" to represent the range of 1 to 3). | String
validation | No | Whether the answer must pass certain validation parameters | Object
validation.type | Yes | Type of validation to use: "any", "integer", "decimal", "date_us", "date_intl", "regex", "email", or "text_length" | String
validation.text | Yes | Text to display if validation fails | String
validation.min | Yes | Minimum value an answer can be to pass validation | Date String, int, or null depending on validation.type
validation.max | Yes| Maximum value an answer can be to pass validation | Date String, int, or null depending on validation.type
validation.sum | No | Only accepted is family=open_ended and subtype=numerical, the exact integer textboxes must sum to in order to pass validation | Integer
validation.sum_text | No | Only accepted is family=open_ended and subtype=numerical, the message to display if textboxes do not sum to validation.sum | String
forced_ranking | No | Required if type is matrix and subtype is rating or single, whether or not to force ranking | Boolean
answers |Yes  for all question types except open_ended_single| Answers object, refer to [Formatting Question Types](#formatting-question-types) | Object

####Response Resource

Name  |Description | Type
----- |------ | -----
headings  |Question heading | List
headings[\_].heading  |The title of the question, or empty string if `random_assignment` is defined | String
headings[\_].description  | If `random_assignment` is defined, and `family` is `presentation_image` this is the title | String
headings[\_].image  |Image data when question `family` is `presentation_image`| Object or null
headings[\_].image.url  | URL of image when question `family` is `presentation_image`| String
headings[\_].random_assignment  |Random assignment data | Object or null
headings[\_].random_assignment.percent  |Percent chance of this random assignment showing up (must sum to 100) | Integer
headings[\_].random_assignment.position  |Position of the random_assignment in survey creation page | Integer
headings[\_].random\_assignment.variable\_name  |Internal use name for question tracking, can be `""` | String
headings[\_].random\_assignment.id  |Internal use id for question tracking | String
position  | Position of question on page | Integer
visible  | Whether the question is visible (corresponds with being deleted in the UI) | Boolean
family  | Question type, see [formatting question types](#formatting-question-types) | String
subtype  | Question type's subtype, see [formatting question types](#formatting-question-types)  | String
sorting  | Sorting details of answers | Object
sorting.type  | Sort answer choices by: "default", "textasc", "textdesc", "resp_count_asc", "resp_count_desc", "random", "flip"| String
sorting.ignore_last | Do not sort the last answer option (useful for "none of the above", etc) | Boolean
required  | Whether an answer is required for this question | Object
required.text  | String to display if a required question is not answered | String
required.type  | Required type: "all" , "at_least", "at_most", "exactly", or "range" | String
required.amount  | The amount of answers required to be answered. If the required type is range then this is two numbers separated by a comma, as a string (e.g. "1,3" to represent the range of 1 to 3). | String
validation  | Whether the answer must pass certain validation parameters | Object
validation.type  | Type of validation to use: "any", "integer", "decimal", "date_us", "date_intl", "regex", "email", or "text_length" | String
validation.text  | Text to display if validation fails | String
validation.min  | Minimum value an answer can be to pass validation | Date String, int, or null depending on validation.type
validation.max | Maximum value an answer can be to pass validation | Date String, int, or null depending on validation.type
validation.sum  | Only accepted is family=open_ended and subtype=numerical, the exact integer textboxes must sum to in order to pass validation | Integer
validation.sum_text  | Only accepted is family=open_ended and subtype=numerical, the message to display if textboxes do not sum to validation.sum | String
forced_ranking  | Required if type is matrix and subtype is rating or single, whether or not to force ranking | Boolean
answers | Answers object, refer to [Formatting Question Types](#formatting-question-types) | Object
id | Question id | String

###/surveys/{id}/pages/{id}/questions/{id}

>Definition

```
GET https://api.surveymonkey.net/v3/surveys/{survey_id}/pages/{page_id}/questions/{question_id}
```

>Example Request

```shell
curl -i -X POST -H "Authorization:bearer YOUR_ACCESS_TOKEN" https://api.surveymonkey.net/v3/surveys/{survey_id}/pages/{page_id}/questions/{question_id}?api_key=YOUR_API_KEY 

```

```python
import requests

s = requests.session()
#See Requests Guide for Setting Up Requests Session

url = "https://api.surveymonkey.net/v3/surveys/%s/pages/%s/questions/%s" % (survey_id, page_id, question_id, YOUR_API_KEY)
s.get(url)

####Response

Same as `POST` [/surveys/{id}/pages/questions](#surveys-id-pages-id-questions)

```

####Available Methods

 * `HEAD`: Checks if resource is available
 * `OPTIONS`: Returns available methods and options
 * `GET`: Returns a question 
 * `PATCH`: Updates a question (updates any fields accepted as arguments to `POST` [/surveys/{id}/pages/questions](#surveys-id-pages-id-questions))
 * `PUT`: Replaces a question (same arguments and requirements as `POST` [/surveys/{id}/pages/questions](#surveys-id-pages-id-questions))
 * `DELETE`: Deletes a question

###Formatting Question Types

All questions have a `family` and `subtype` that define their type. See below for example formatting of the answers object for different questions types. Read more about SurveyMonkey's question types in our [help center](http://help.surveymonkey.com/articles/en_US/kb/Available-question-types-and-formatting-options). 

|Family|Subtype|
|------|-------|
|single_choice|'vertical'|
|matrix| 'single', 'rating', 'ranking', 'menu'|
|open_ended|'single','multi', 'numerical'|
|demographic|'international'|
|datetime|'both'|

####Single Choice

>Single Choice

```json
{
    "headings": [
        {
            "heading": "Which monkey would you rather have as a pet?"
        }
    ],
    "position": 1,
    "family": "single_choice",
    "subtype": "vertical",
    "answers": {
        "choices":[
            {
                "text": "Capuchin"
            },
            {
                "text": "Mandrill"
            }
        ]
    }
}
```
![Single Choice](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/single-choice.png)

Name | Description | Type
----- | ------ | -----
choices (required) | List of available choices for the user | List
choices[\_].text (required) | Choice for user selection | String
choices[\_].position (optional) | Position of the current choice | Integer

####Multiple Choice

>Multiple Choice

```json
{
    "headings": [
        {
            "heading": "Which monkeys would you like as pets?"
        }
    ],
    "position": 1,
    "family": "multiple_choice",
    "subtype": "vertical",
    "answers": {
        "choices":[
            {
                "text": "Capuchin"
            },
            {
                "text": "Mandrill"
            }
        ]
    }
}
```
![Multiple Choice](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/multiple-choice.png)

Name | Description | Type
----- | ------ | -----
choices (required) | List of available choices for the user | List
choices[\_].text (required) | Choice for user selection | String
choices[\_].position (optional) | Position of the current choice | Integer

####Matrix - Single

>Matrix - Single

```json
{
    "headings": [
        {
            "heading": "What is each of your family members' favorite ice cream?"
        }
    ],
    "position": 1,
    "family": "matrix",
    "subtype": "single",
    "forced_ranking": false,
    "answers": {
        "rows": [
            {
                "text": "Mother"
            },
            {
                "text": "Father"
            }
        ],
        "choices": [
            {
                "text": "Chocolate"
            },
            {
                "text": "Vanilla"
            },
            {
                "text": "Strawberry"
            }
        ]
    }
}
```
![Matrix Single](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/matrix-single.png)

Name | Description | Type
----- | ------ | -----
rows (required) | List of rows in the matrix | List
rows[\_].text (required) | Text label for the row | String
rows[\_].position (optional) | Position of the row | Integer
choices (required) | List of available choices for the user | List
choices[\_].text (required) | Choice for user selection | String
choices[\_].position (optional) | Position of the current choice | Integer

####Matrix - Rating

>Matrix - Rating

```json
{
    "headings": [
        {
            "heading": "What is each of your family members' favorite ice cream?"
        }
    ],
    "position": 1,
    "family": "matrix",
    "subtype": "rating",
    "forced_ranking": false,
    "answers": {
        "rows": [
            {
                "text": "Mother"
            },
            {
                "text": "Father"
            }
        ],
        "choices": [
            {
                "text": "Chocolate",
                "weight": 1
            },
            {
                "text": "Vanilla",
                "weight": 1
            },
            {
                "text": "Strawberry",
                "weight": 1
            }
        ]
    }
}
```
![Matrix Rating](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/matrix-rating.png)

Name | Description | Type
----- | ------ | -----
rows (required) | List of rows in the matrix | List
rows[\_].text (required) | Text label for the row | String
rows[\_].position (optional) | Position of the row | Integer
choices (required) | List of available choices for the user | List
choices[\_].text (required) | Choice for user selection | String
choices[\_].weight (required) | Weight value of the choice | Integer
choices[\_].position (optional) | Position of the row | Integer

####Matrix - Ranking

>Matrix - Ranking

```json
{
    "headings": [
        {
            "heading": "Rank these flavors in terms of your preference!"
        }
    ],
    "position": 1,
    "family": "matrix",
    "subtype": "ranking",
    "answers": {
        "rows":[
            {
                "text": "Vanilla"
            },
            {
                "text": "Chocolate"
            },
            {
                "text": "Strawberry"
            }
        ]
    }
}
```
![Matrix Ranking](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/matrix-ranking.png)

Name | Description | Type
----- | ------ | -----
rows (required) | List of rows in the matrix | List
rows[\_].text (required) | Text label for the row | String
rows[\_].position (optional) | Position of the row | Integer

####Matrix - Menu

>Matrix - Menu

```json
{
    "headings": [
        {
            "heading": "Which texture and taste do you prefer on your ice cream?"
        }
    ],
    "position": 1,
    "family": "matrix",
    "subtype": "menu",
    "answers": {
        "rows":[
            {
            "text": "Chocolate"
            },
            {
            "text": "Cookies and Cream"
            }
        ],
        "cols":[
            {
                "text": "Texture",
                "choices": [
                    {
                        "text": "Smooth",
                        "visible": true,
                        "position": 1
                    },
                    {
                        "text": "Chunky",
                        "visible": true,
                        "position": 2
                    }
                ]
            },
            {
                "text": "Flavor",
                "choices": [
                    {
                        "text": "Light",
                        "visible": true,
                        "position": 1
                    },
                    {
                        "text": "Full",
                        "visible": true,
                        "position": 2
                    }
                ]
            }
        ]
    }
}
```
![Matrix Menu](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/matrix-menu.png)

Name | Description | Type
----- | ------ | -----
rows (required) | List of rows in the matrix | List
rows[\_].text (required) | Text label for the row | String
rows[\_].position (optional) | Position of the row | Integer
cols (required) | List of columns in the matrix | List
cols[\_].text (required) | Text label for column | String
cols[\_].choices (required) | List of available choices for the user in dropdown menu | List
cols[\_].choices[\_].text (required) | Choice for user selection | String
cols[\_].choices[\_].position (required) | Position of choice | Integer


####Open Ended - Single or Essay

>Open Ended - Single or Essay

```json
{
    "headings": [
        {
            "heading": "What's your favorite thing about monkeys?"
        }
    ],
    "position": 1,
    "family": "open_ended",
    "subtype": "single"
}
```
![Open Ended Single](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/open-ended-single.png)

Only requires a `heading`.

####Open Ended - Multi

>Open Ended - Multi

```json
{
    "headings": [
        {
            "heading": "What are your favorite monkey species?"
        }
    ],
    "position": 1,
    "family": "open_ended",
    "subtype": "multi",
    "answers": {
        "rows":[
            {
                "text": "Your favorite:"
            },
            {
                "text": "Second favorite:"
            }
        ]
    }
}
```
![Open Ended Multi](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/open-ended-multi.png)

Name | Description | Type
----- | ------ | -----
rows (required) | List of textboxes | List
rows[\_].text (required) | Text label for textbox | String
rows[\_].position (optional) | Position of the current row | Integer

####Open Ended - Numerical

>Open Ended - Numerical

```json
{
    "headings": [
        {
            "heading": "If you could split 10 pounds of ice cream, how would you split it?"
        }
    ],
    "position": 1,
    "family": "open_ended",
    "subtype": "numerical",
    "answers": {
        "rows":[
            {
                "text": "Pounds for family:"
            },
            {
                "text": "Pounds for friends:"
            },
            {
                "text": "Pounds for self:"
            }
        ]
    },
    "validation": {
        "text": "Please enter a valid integer.",
        "type": "integer",
        "sum": 10,
        "sum_text": "Your input must add up to 10 pounds!"
    }
}
```
![Open Ended Numerical](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/open-ended-numerical.png)

Name | Description | Type
----- | ------ | -----
rows (required) | List of textboxes | List
rows[\_].text (required) | Text label for textbox | String
rows[\_].position (optional) | Position of the current row | Integer


####Demographic

>Demographic

```json
{
    "headings": [
        {
            "heading": "What's your address?"
        }
    ],
    "position": 1,
    "visible": true,
    "family": "demographic",
    "subtype": "international",
    "answers": {
        "rows": [
            {
                "visible": true,
                "required": true,
                "type": "name",
                "text": "Your Name"
            },
            {
                "visible": true,
                "required": false,
                "type": "company"
            },
            {
                "visible": false,
                "required": false,
                "type": "address"
            },
            {
                "visible": false,
                "required": false,
                "type": "address2"
            },
            {
                "visible": true,
                "required": true,
                "type": "city"
            },
            {
                "visible": true,
                "required": true,
                "type": "state"
            },
            {
                "visible": true,
                "required": true,
                "type": "zip"
            },
            {
                "visible": true,
                "required": true,
                "type": "country"
            },
            {
                "visible": true,
                "required": true,
                "type": "email"
            },
            {
                "visible": true,
                "required": true,
                "type": "phone"
            }
        ]
    }
}
```
![Demographic](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/demographic.png)

This corresponds to the **Contact Information** question type in the SurveyMonkey UI.

Each `type` represented in the example object must be included, to disable one, specify `visible` as `False`

Name | Description | Type
----- | ------ | -----
rows | List of available demographic data | Object
rows[\_].visible (required) | Visibility of demographic data type | Boolean
rows[\_].required (required) | Whether demographic data type is required | Boolean
rows[\_].type (required) | Type of demographic data | String
rows[\_].text (optional) | Optional label of demographic data, will default to type | String


####DateTime

>DateTime

```json
{
    "headings": [
        {
            "heading": "When did you last eat ice cream?"
        }
    ],
    "position": 1,
    "visible": true,
    "family": "datetime",
    "subtype": "both",
    "answers": {
        "rows": [
            {
                "text": "At:",
                "position": 1
            }
        ]
    }
}
```
![DateTime](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/datetime.png)

Name | Description | Type
----- | ------ | -----
rows[\_].text (required) | Label for date/time input box | String
rows[\_].position (optional) | Position of date/time input box | Integer

####Presentation

>Presentation

```json
{
    "headings": [
        {
            "heading": "This is a monkey",
            "image": {
                "image_url": "http://surveymonkey.com/monkey.jpg"
            }
        }
    ],
    "position": 1,
    "family": "presentation",
    "subtype": "descriptive_text"
}
```
![DateTime](https://raw.githubusercontent.com/SurveyMonkey/public_api_docs/master/images/datetime.png)

If `image` is included, this corresponds to the **Image** question type in the SurveyMonkey UI. If not included, it corresponds to the **Text** question type. 

Name | Description | Type
----- | ----- | -----
 image (optional)| Image to present|dictionary
 image[\_].image_url (required)| URL of image to present | String |
