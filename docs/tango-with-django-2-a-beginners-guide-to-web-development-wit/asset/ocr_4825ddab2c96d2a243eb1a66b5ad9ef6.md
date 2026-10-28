```
```text
$ python populate_rango.py

Starting Rango population script...
- Python: Official Python Tutorial
- Python: How to Think Like a Computer Scientist
- Python: Learn Python in 10 Minutes
- Django: Official Django Tutorial
- Django: Django Rocks
- Django: How to Tango with Django
- Other Frameworks: Bottle
- Other Frameworks: Flask
```
```

Next, verify that the population script worked as it should have and populated the database. To do this, restart the Django development server, and navigate to the admin interface (at `http://127.0.0.1:8000/admin/`). Once there, check that you have some new categories and pages. Do you see all the pages if you click Pages, like in the figure shown below?

![](bbox=[0.153, 0.479, 0.849, 0.814])

**The Django admin interface, showing the Page model populated with the new population script. Success!**