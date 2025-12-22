##### Install Python + spaCy using Miniconda (Windows setup) #####

# These instructions will have you:
#    1. Install a tool called Miniconda (it manages Python environments)
#    2. Create a separate Python environment just for spaCy (so nothing conflicts)
#    3. Install spaCy and the English language model
#    4. Tell R’s spacyr package to use that Python


### Step 0: Close RStudio (recommended)
#    If RStudio is open, close it. This prevents R from “locking onto” an old Python during setup.


### Step 1: Install Miniconda
#    1. Open your web browser and search: “Miniconda download Windows”
#    2. Download Miniconda3 Windows 64-bit (x86_64).
#    3. Run the installer.
#    4. During install:
#        -Choose Just Me (easiest)
#        -Keep the default install location
#        -If you see an option like “Add Miniconda to PATH”: it’s okay either way. (Not required if you always use Anaconda Prompt, which you will).
#    5. When finished, you should have an app called “Anaconda Prompt” in your Start menu.


### Step 2: Open Anaconda Prompt
#    1. Click the Windows Start button
#    2. Type: Anaconda Prompt
#    3. Open it. You’ll see a black terminal window with text.


## Step 3: Create a Python environment for spaCy
#    1. In **Anaconda Prompt**, copy/paste this and press Enter:
```conda create -n spacyr-env python=3.11 -y
```
#    2. What this does:
        -This creates a new environment named spacyr-env with Python 3.11.
        -Creates a new environment named spacyr-env
        -Installs Python 3.11 inside it. This may take a minute. You’ll see a lot of text scrolling—normal.


### Step 4: Activate the environment
#    1. Still in Anaconda Prompt, run:
```
conda activate spacyr-env
```
#    2. When it’s activated, you should see something like this at the start of the line:
```
(spacyr-env) C:\...
```
#    3. That (spacyr-env) is important. It means you’re installing things into the right place.


### Step 5: Install spaCy
#    1. Now install spaCy:
```
python -m pip install -U pip
python -m pip install spacy
```
#    2. If it prints a lot of download/install messages, that’s good.


### Step 6: Install the English language model
#    1. spaCy needs a language model to do parsing. Run:
```
python -m spacy download en_core_web_sm
```
#    2. Success looks like: “Successfully installed en_core_web_sm …” (wording varies).


### Step 7: Find the exact Python path you just set up
#    1. This matters because you’ll paste it into R. In Anaconda Prompt, run:
```
where python
```
#    2. You’ll see one or more paths. You want the one that includes:
```
...\envs\spacyr-env\python.exe
```
#    3. For example:
```
C:\Users\Vivian\miniconda3\envs\spacyr-env\python.exe
```

### Step 8: 









