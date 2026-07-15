# Student-Information-Dash-App
One of my first Dash creations 



Here are the packages we will need to start planning out our dashboard:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import plotly.express as px
import dash
from dash import Dash, html, dash_table, dcc
from dash.dependencies import Input, Output
import dash_bootstrap_components as dbc
```

Now load in the data: 
```python
students = pd.read_csv("./student-por.csv")
students
``` 
<img width="1990" height="800" alt="image" src="https://github.com/user-attachments/assets/4cdfa2b9-41c0-4db5-859d-29c3b2a21243" />

Now we can start creating figures that we'll use in the end result dashboard. Let's start by creating a visualization to show the association between absences and the students' final grades, and we can hue it by sex. (Keep in mind we are doing this via plotly, which very much follows the matplotlib style): 
```python
fig = px.scatter(
    students, x='absences', y='G3', color='sex', color_discrete_map={
        'F': 'hotpink',
        'M': 'dodgerblue'})
fig
``` 
Resulting in: 
<img width="1974" height="630" alt="image" src="https://github.com/user-attachments/assets/aab7e528-d80c-4d47-b83f-4c88d33b4080" />


Now let's get something relating to the students' parents. We have the parents' basic occupations. Let's see how they relate to the average term grade. We can view this in a confusion matrix for clarity:
```python
grouped_absences = students.groupby(['Mjob', 'Fjob'], as_index=False)[['G1', 'G2', 'G3']].mean()

new_heatmap = px.density_heatmap(
    grouped_absences,
    x='Mjob',
    y='Fjob',
    z='G1',
    histfunc='avg',
    color_continuous_scale='RdBu'
)

new_heatmap.update_layout(
    title="Average G1 by Mother's Job and Father's Job",
    xaxis_title="Mother's Job",
    yaxis_title="Father's Job",
    coloraxis_colorbar=dict(
        title="Average Grade",
        ticks="outside"
    )
)

new_heatmap
``` 
Resulting in: 
<img width="1974" height="646" alt="image" src="https://github.com/user-attachments/assets/03c205af-a7b3-4a9c-a87c-25269806937c" />


And for our third figure, we have access to alcohol consumption data, so let's see how that has affected grades:

```python
agg = students.groupby(['Dalc','sex'], as_index=False)['absences'].mean()
agg['absences'] = agg['absences'].round(2)

fig4 = px.bar(
    agg,
    x='Dalc',         # weekday alcohol level
    y='absences',     # average absences
    color='sex',      # split by sex
    barmode='group',  # side-by-side bars (paired)
    labels={'Dalc':'Weekday Alcohol Consumption (1=low,5=high)',
            'absences':'Average Absences'},
    text='absences' 
)

fig4.update_layout(
    title="Average Absences by Weekday Alcohol Consumption and Sex",
    xaxis=dict(type='category')  # ensure Dalc treated as categorical
)

fig4.show()
``` 
Results in: <img width="1958" height="654" alt="image" src="https://github.com/user-attachments/assets/ff8082be-a3fe-4e0d-b2e9-3b2f21bda1ce" />


And now we can move to actually building the structure of our dash app layout:

TBC!









