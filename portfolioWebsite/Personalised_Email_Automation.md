# Personalised Email Automation

Sends personalised emails automatically from CSV file using Python.

---
## Example 
<img width="777" alt="Personalised_Email_Automation" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/2c0bac05-5223-46bd-beb1-fe8f2f29b77f">

## Project Code
```PYTHON
import datetime as dt
import pandas as pd
import os
import random
import smtplib

birthdays_df = pd.read_csv('birthdaysEmailList.csv')
now = dt.datetime.now()

# get today birthday star by checking if today matches a birthday in the csv file
birthday_star = birthdays_df.loc[(birthdays_df.month == now.month) & (birthdays_df.day == now.day)]

if not birthday_star.empty:
    # pick a random letter from letter templates
    letters_dir = 'letter_templates/'
    letter_file = random.choice(os.listdir(letters_dir))

    with open(f'{letters_dir}{letter_file}','r') as file:
        letter = file.read()

    # replace the [NAME] with the person's actual name from the csv file
    letter = letter.replace('[NAME]', birthday_star.name.to_string(index= False))

    # send email
    with smtplib.SMTP('smtp.gmail.com') as connection:
        connection.starttls()
        connection.login(user= MY_EMAIL, password= PASSWORD)
        connection.sendmail(
            from_addr= MY_EMAIL,
            to_addrs= birthday_star.email.to_string(index= False),
            msg= f'Subject:Love for your big day!\n\n{letter}'
        )
```
