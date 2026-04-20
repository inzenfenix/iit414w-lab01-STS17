# Tomás Solano, InZenFenix, IIT414W, 20-04-2026
 
---

# System Information
- OS: CachyOS **6.19.5-3**
- Python **3.14.3**
- pip **26.0.1**

---

---

# Important information

If you want to get the results from lab 1, go to that folder, also I added a new notebook called "baseline_plus.ipynb" with the Majority-Class model in there with the obtained data.

---
# Setup Instructions

## 1. Open a terminal
Open **CMD**, **PowerShell**, or **bash** (Linux/macOS).

## 2. Navigate to the project directory
Move to the folder where the project is located. Example:

```bash
cd C:/Users/[Your_username]/Documents/Github/iit414w-lab01-inzenfenix
```

## 3. Create a Python virtual environment:
``` bash
python -m venv .venv
```

If that command doesn't work try:

``` bash
python3 -m venv .venv
```

> The environment is named .venv so it stays hidden from git repositories.

## 4. Activate the virtual environment:
``` bash
source .venv/bin/activate
```

> Fish shell (used by some Linux systems like CachyOS):
``` bash
source .venv/bin/activate.fish
```

> PowerShell
``` bash
source .venv/bin/activate.ps1
```

> Windows CMD
``` bash
.venv\Scripts\activate
```

> After activation, your terminal should show (.venv) at the beginning of the prompt.

## 5. Install project dependencies
``` bash
pip install -r requirements.txt
```

## 6. Start JupyterLab:
``` bash
jupyter lab
```

> JupyterLab will usually open automatically in your browser.
> If it does not, the terminal will show a URL similar to this:
``` bash
http://localhost:8888/lab?token=6bdadd316fe5eb2941e8797c93d90e4233a49ba6e3c82dc0
```
> Copy and paste this link into your browser to access the notebook interface.

---

# How to run

> Once inside jupyter's lab webpage, you can double click or press right click and then *Open* to get into any **.ipynb** file to open it.

![image](images/Opening_Notebook.png)

> [!IMPORTANT]
> You should run the entirety of *setup_check.ipynb* at least once to make sure you have the right setup, then you can start *data_check.ipynb*

> To start a cell you can press the play button, do this for each cell in order from top to bottom of a notebook.

![image](images/Start_Cell.png)

# Problems encountered

Some issues I ran into where:
1. When flattening the JSON data from the Jolpica API, since when the data is given you can't immediately format it to a DataFrame, so I had to create a small system to automatize its formatting.
2. Formatting deltatime64 into something more readable, since when pandas uses this type of data structure it gives it as "X days HH:MM:SS ns" it gets kind of hard to read so I needed to create a function that could get me that data in just seconds

# Expected Outputs

If once you run each cell of both notebooks you don't run into any errors and can see that the checks for the different scripts have succesfuly passed then the systems are running and you are ready to go into testing more about F1!