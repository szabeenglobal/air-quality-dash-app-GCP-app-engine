
This guide shows how to create and deploy a demo dash application using Google Cloud Platform (GCP) App Engine, using data file located in a github repository. The commits that you push to your Github repository automatically are automatically copied into a repository hosted in Cloud Source Repositories instead of your local computer. 
Here is the [link](https://air-quality-dash-app.ew.r.appspot.com)  to the demo dash app that we built using this guide. 

Please note that this guide is work in progress. 

The steps involved are: 

## Step 1: Create a dash application

In order to creat a dash app you will need four files: main.py, app.yaml, requirements.txt, .gcloudignore, and two folders: data, and assets. 

**main.py** 
The main.py file should at least contain the following section 

**Import statements** 
**Python functions** - if you are using any function for data analysis or visualization<br>
**Load data**<br>
**Text** <br>
**Dash** - This is the core of a dash application. 
```python
app.layout = html.Div(children=[
    html.H1(
        "Air quality data visualization"),

    dbc.Container(
        dbc.Row(
            [dbc.Col(dcc.Graph(id='graph'), width=6),
            dbc.Col(dcc.Graph(id='graph-1'), width=6)]
        )
    )
    ,
    
    html.Label([
        "colorscale", 
        dcc.Dropdown(
            id='colorscale-dropdown',
            clearable=False,
            value='plasma',
            options=[{'label':c, 'value':c}
                    for c in px.colors.named_colorscales()
                    ])
    ])

])
```
**Callbacks** - to add interactivity to your dash app. 


**requirements.txt**
```python
Click==7.0
dash==1.20.0
plotly==4.14.3
pandas==1.1.0
numpy==1.19.5
dash-core-components==1.16.0
dash-html-components==1.1.3
dash_bootstrap_components==0.12.1

dash-renderer==1.2.0
Flask==1.1.1
Flask-Compress==1.4.0
future==0.18.2
itsdangerous==1.1.0
Jinja2==2.10.3
MarkupSafe==1.1.1

pytz==2019.3
retrying==1.3.3
six==1.13.0
Werkzeug==0.16.0
gunicorn>=19.5.0
google-api-core==1.16.0
google-auth==1.10.1
google-cloud-core==1.2.0
google-cloud-storage==1.25.0
google-resumable-media==0.5.0
googleapis-common-protos==1.51.0
```
**app.yaml**
This is an important file that configures the App Engine's settings. The first line should be 'service: default" but this gives error, if this is your first app deployment on App ENgine. Do not add it. The second line is the runtime which is python 37. The basic_scaling and resources defines the App engine environment. Here,  we are using maximum instances to 1, on a machine with 1 CPU and 1 GB of RAM. the last line define's app's entrypoint. 
```python
runtime: python37
env: standard 

basic_scaling:
    max_instances: 1
    idle_timeout: 10m

resources:
    cpu: 1
    memory_gb: 1
    disk_size_gb: 10

entrypoint: gunicorn -b 0.0.0.0:8080 main:server
```


## Step 2: Create a new repo on github and push the app to GitHub

 **2.1** Create a new repository on github. Copy the https link to your repo. The link to this demo app's repo is https://github.com/szabeenglobal/air-quality-dash-app-GCP-app-engine
 **2.2** Go to Terminal and run the following commands. 
```
git init
git add .
git commit -m "initial commit"
git remote add origin [github-repo-HTTPS-link]
git push -u origin master
```

## Step 3: Deploy your Application to Google Cloud Platform App engine 
 **3.1** Step Mirror the app repository to Cloud Source Repositories
**3.2** https://cloud.google.com/source-repositories/docs/mirroring-a-github-repository
 **3.3** Enable Cloud Build API and App  Engine Admin API
**3.4** Create an App Engine application
**3.4** Add trigger in Cloud Build



