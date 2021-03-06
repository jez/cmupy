# cmupy
**cmupy** is a Python library for CMU data. Scheudling API data uses the [ScottyLabs CMU APIs](https://apis.scottylabs.org).
### Table of Contents
* [Scheduling](#scheduling)
* [Directory](#directory)
* [Printer Status](#printer-status)

## APIs
To use this library, you must be affiliated with CMU. Register your application with [apis@cmu](https://apis.scottylabs.org/apps) to get an app ID and an app secret key. Then, to use the library, you simply need to include `apis.py` in your app.

<a name="scheduling"/>
### Scheduling

```python
>>> import apis
>>> schedule = apis.Scheduling(app_id='YOUR_APP_ID', app_secret_key='YOUR_SECRET_KEY')
>>> schedule.departments(semester='S14')
[
  {u'id': 31, u'name': u'Aerospace Studies-ROTC'},
  {u'id': 48, u'name': u'Architecture'},
  .
  .
  .
  {u'id': 47, u'name': u'Tepper School of Business'}
]
>>> schedule.courses(semester='S14', department=15) # fairly slow
[
  {
    u'number': u'15090',
    u'name': u'Computer Science Practicum',
    u'department_id': 15,
    .
    .
    .
  },
  .
  .
  .
]

>>> # the following are equivalent
>>> schedule.course(semester='S14', course_number=15090)
>>> schedule.course(semester='S14', course_number='15090')
>>> schedule.course(semester='S14', department=15, course_id=90)
>>> schedule.course(semester='S14', department='15', course_id='090')
{
  u'units': 3,
  u'lectures': [
    {
      u'time_start': u'TBA',
      u'section': u'A',
      u'instructors': u'Cortina',
      u'days': u'TBA',
      u'time_end': u'TBA',
      u'location': u'DNM DNM',
      u'meetings': [
        {
          u'location': u'DNM DNM',
          u'time_start': u'TBA',
          u'days': u'TBA',
          u'time_end':
          u'TBA'
        }
      ]
    }
  ],
  u'number': u'15090',
  u'name': u'Computer Science Practicum',
  u'department_id': 15
}
```

You can omit the semester parameter to use the current semester's data by default. Department IDs, course numbers, and course IDs can be either strings or integers.

More documentation can be found in [docs.html](https://rawgithub.com/tomshen/cmupy/master/cmu.html).

<a name="directory"/>
## Directory
To use this library, you need to install [Beautiful Soup 4](http://www.crummy.com/software/BeautifulSoup/) (`pip install beautifulsoup4`) as a dependency, and include `directory.py` in your app.

```python
>>> from directory import Directory
>>> Directory.get_info(andrewid='zhixians')
{
  u'Website': u'...',
  u'Andrew UserID': u'...',
  u'Student Class Level': u'...',
  u'On Campus': u'...',
  u'Department with which this person is affiliated': u'...',
  u'Advisor': u'...',
  u'Job Title According to HR': u'...',
  u'Preferred Name': u'...',
  u'Email': u'...'
}
>>> Directory.get_info('tom')
[
  {
    u'Last Name': u'...',
    u'First Name': u'Tom',
    u'Andrew UserID': u'...',
    u'Department': u'...',
    u'Affiliation': u'...'
  },
  .
  .
  .
]
```

If you provide an Andrew ID, `get_info` will raise an exception if it's not a valid Andrew ID that uniquely indentifies a person, or return a dictionary containing information about the person identified by that Andrew ID.

If you otherwise provide a query, `get_info` will return a list of all the people who match that query, or an empty list if no people match that query. You can than obtain more information about each person by looking up that person individually by Andrew ID.

<a name="printer-status"/>
## Printer Status
To use this library, you need to install [Beautiful Soup 4](http://www.crummy.com/software/BeautifulSoup/) (`pip install beautifulsoup4`) as a dependency, and include `printing.py` in your app.

```python
>>> import printing
>>> ps = printing.Status()
>>> ps.get_status('gates 3')
{'status': 'ready', 'details': 'ready'}
>>> ps.get_all_statuses()
{
  'clusters - cyert b&w': {'status': 'ready', 'details': 'ready'},
  'housing - mudge b&w': {'status': 'caution', 'details': 'tray 3 empty plain letter   ready'},
  .
  .
  .
  'library - hunt 1 ref 1 b&w': {'status': 'not ready', 'details': 'no response'}
}
```

You can look up a printer by calling `get_status` using a string containing any part of a printer's name. For example, `'Mudge'`, `'Mudge B&W'`, and `'housing - mudge b&w'` will all return the status of the printer in Mudge.
