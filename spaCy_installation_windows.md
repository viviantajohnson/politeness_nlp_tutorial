# Install Python + spaCy using Miniconda (Windows setup)

## These instructions will have you:
1. Install a tool called Miniconda (it manages Python environments)
2. Create a separate Python environment just for spaCy (so nothing conflicts)
3. Install spaCy and the English language model
4. Tell R’s spacyr package to use that Python


## Step 0: Close RStudio (recommended)
If RStudio is open, close it. This prevents R from “locking onto” an old Python during setup.


## Step 1: Install Miniconda
1. Open your web browser and search: **Miniconda download Windows**
2. Download **Miniconda3 Windows 64-bit** (x86_64).
3. Run the installer.
4. During install:  
   - Choose **Just Me** (easiest)  
   - Keep the default install location  
   - If you see an option like **Add Miniconda to PATH:** it’s okay either way. (Not required if you always use Anaconda Prompt, which you will).  
5. When finished, you should have an app called **Anaconda Prompt** in your Start menu.


## Step 2: Open Anaconda Prompt
1. Click the Windows **Start** button
2. Type: **Anaconda Prompt**
3. Open it. You’ll see a black terminal window with text.


## Step 3: Create a Python environment for spaCy
In **Anaconda Prompt**, copy/paste this and press Enter:
```
conda create -n spacyr-env python=3.11 -y
```
What this does:  
- This creates a new environment named ```spacyr-env``` with Python 3.11.  
- Creates a new environment named ```spacyr-env```  
- Installs Python 3.11 inside it. This may take a minute. You’ll see a lot of text scrolling—normal.


## Step 4: Activate the environment
Still in **Anaconda Prompt**, run:
```
conda activate spacyr-env
```
When it’s activated, you should see something like this at the start of the line:
```
(spacyr-env) C:\...
```
That ```(spacyr-env)``` is important. It means you’re installing things into the right place.


## Step 5: Install spaCy
Now install spaCy:
```
python -m pip install -U pip
python -m pip install spacy
```
If it prints a lot of download/install messages, that’s good.


## Step 6: Install the English language model
spaCy needs a language model to do parsing. Run:
```
python -m spacy download en_core_web_sm
```
Success looks like: “Successfully installed en_core_web_sm …” (wording varies).


## Step 7: Find the exact Python path you just set up
This matters because you’ll paste it into R. **In Anaconda Prompt**, run:
```
where python
```
You’ll see one or more paths. You want the one that includes:
```
...\envs\spacyr-env\python.exe
```
For example:
```
C:\Users\Vivian\miniconda3\envs\spacyr-env\python.exe
```
Copy that full path.


## Step 8: Open RStudio and point ```spacyr``` to that Python
Now open RStudio. Run this in the R console (paste your path):
```
install.packages("spacyr")
install.packages("reticulate")  # helps R talk to Python (useful for debugging)

library(spacyr)

spacy_initialize(
  python_executable = "C:/Users/YourName/miniconda3/envs/spacyr-env/python.exe"
)
spacy_version()
spacy_get_langmodels()
```
Important Windows note: In R strings, use **forward slashes** (/). 
So change this ```C:\Users\...``` to this ```C:/Users/...```

What success looks like:
   - ```spacy_version()``` prints a version number
   - ```spacy_get_langmodels()``` lists ```en_core_web_sm```

## Step 9: Run a quick “smoke test” parse
In R, run:
```
spacy_parse("Thanks so much for your help — I really appreciate it.")
```
If you get a data frame of tokens back, parsing is working.


## Step 10: Use ```politeness``` with spaCy parsing
Now you can run ```politeness()``` with ```parser = "spacy"```:
```
library(politeness)

texts <- c(
  "Thanks so much for your time!",
  "You need to fix this. This is unacceptable."
)

feat <- politeness(
  text = texts,
  parser = "spacy",
  metric = "count"
)

feat
```

# Troubleshooting

## **Problem:** ```spacy_initialize()``` fails
Run this in R:
```
reticulate::py_config()
```
If the Python shown is NOT your ```spacyr-env```, force it:
```
reticulate::use_python("C:/Users/YourName/miniconda3/envs/spacyr-env/python.exe", required = TRUE)
library(spacyr)
spacy_initialize()
```


## **Problem:** ```spacy_get_langmodels()``` is empty
You probably installed spaCy but not the model (or installed it into the wrong environment).

Go back to **Anaconda Prompt***:
```
conda activate spacyr-env
python -m spacy download en_core_web_sm
```
Then restart RStudio and try again.


## **Problem:** You don’t have Anaconda Prompt  
You installed Miniconda, but Windows search isn’t finding it. Reboot your computer and search again. If still missing, reinstall Miniconda (and keep default install options).




