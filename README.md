# Spoon Calculator
An Kivy app that tracks energy expenditure throughout the day and helps users identify correlations between behaviour and energy.

This app is aimed for people with limited energy, who struggle to recognise (and thus not overstep) their limits, since the consequenting symptoms for overstepping these limits are difficult to measure.

Spoon Calculator allows users to log activities throughout the day, approximating the energy used for each activity using the metaphor of ["spoons"](https://en.wikipedia.org/wiki/Spoon_theory). Users are provided with a plot of the day's energy expenditure so far, as compared with recent days. This gives users an immediate comparison to previous days, a projection for the remainder of the day, and an indication for the variance from day to day.

Equipped with this knowledge, the user will hopefully be able to identify patterns between the type or amount of activities and any severe symptoms they may consequently experience.

## Home screen
The home screen displays a plot of the current day's energy expenditure so far and provides access to all features.

<!--- ![Home screen screenshot](./docs/screenshots/home_screen.png)
-->

<img src="./docs/screenshots/home_screen.png" width="300"/>

The fraction at the top shows "number of spoons spent so far" over "total spoons spent per day averaged over past 14 days".

The plot shows the day's spoon expenditure compared to the past 14 days. The x-axis is the time (beginning and ending at 3am), the y-axis is "spoons". The spoons spent so far today are shown in yellow. The red lines show the spoons spent so far each day, averaged over the past 14 days (thick), plus and minus one standard deviation (thin).

This plot encapsulates a lot of useful information. The yellow line being signicantly above (or below) the thick red line can indicate if the user should rest (or be more active). The thin red lines provide a guide of how far from the mean the yellow line should stray, and also allow for rough extrapolation by eye. The distance between the thin red lines indicate consistency from day to day.

Below the plot are buttons that lead to all available interactions.

## Log an activity
<img src="./docs/screenshots/input_form.png" width="300"/>

This view is used to log activities throughout the day, whether that be every single activity, or just the ones of particular interest to the user. The user may provide an informative name, the "cognitive load", the "physical load", and beginning and end times with a 15 minute resolution. The current energy level may also be provided.

The physical and cognitive loads describe how many spoons an activity costs. Each load can be as "cheap" as 0 spoons per hour, or as expensive as 2 spoons per hour, with an increment of 0.5 spoons per hour between each level. For example, an activity that is very high physical load but only high cognitive load will cost (2 + 1.5) 3.5 spoons per hour. If this activity is performed for two hours, a total of 7 spoons will be logged. It is up to the user to decide where on the scale each activity falls, but to aid consistency graphics are provided. From experience, this scaling system leads to a two digit daily spoon total.

Upon pressing "save" the data will be stored in a locally stored database. This data can be deleted via the "Show logs" view (see below).

## Plot
<img src="./docs/screenshots/weekly_plot.png" width="300"/>

On top of the plot featured in the home screen, various other plots are available. One can choose a time span from daily, weekly or monthly. The daily plots use the same axes units as the plot on the home screen. The weekly and monthly plots' x-axis denote days, with today being 0, yesterday being -1, etc.

## Show logs
Here one can see all the logs stored in the database. All activities for a given day are listed, along with their loads and spoon cost. In the event of an erroneous input, a checkbox at the right of each row can be used to mark an activity for deletion.

## Export logs
The user may export the database as a CSV (comma seperated value) file. The file will be saved in the default android "internal storage" directory with the name "spoon-output.csv". This feature is useful to carry data across reinstallations (see "Import logs") or if the user wishes to analyse the data themself in a more nuanced way.

Warning! This feature does not check for overwrites, so if the database is empty (e.g. you've just reinstalled the app) it will overwrite any previous export.

## Import logs
A previously exported database of logs may be re-imported. The user may provide a custom filename (as a relative path from default android internal storage). The app supplies a default filename which is identical to the one used for exporting.

When importing duplicates are skipped, such that importing the same export twice won't lead to duplicates of every logged activity.

# Build the Android app

Run these commands:

```
python -m venv venv
source ./venv/bin/activate
pip install kivy
pip install buildozer
pip install kivy_garden
pip install setuptools
sudo apt install cython3
sudo apt install openjdk-17-jdk
buildozer android debug
```

The built apk can be found in ./bin/.

### starting over (within a github codespace)...

rm -rf venv
rm -rf ~/.buildozer
rm -rf .buildozer

change in buildozer.spec to
android.accept_sdk_license = True

python -m venv venv
source venv/bin/activate

pip install buildozer
pip install cython

buildozer android debug


## Android packaging with python-for-android

https://kivy.org/doc/stable/guide/packaging-android.html#packaging-with-python-for-android


./sdkmanager "platforms;android-27" --sdk_root=~/android-sdk/

~/android-sdk/cmdline-tools/bin $ ./sdkmanager "build-tools;28.0.2" --sdk_root=~/android-sdk/

sudo apt install android-sdk
export ANDROID_HOME=~/.buildozer/android/android-sdk/



## build the apk (Ubuntu 24.04)

buildozer setup (note this requires libtinfo6 instead of 5):
```
sudo apt update
sudo apt install -y git zip unzip openjdk-17-jdk python3-pip autoconf libtool pkg-config zlib1g-dev libncurses5-dev libncursesw5-dev libtinfo6 cmake libffi-dev libssl-dev
python -m venv venv
source venv/bin/activate
pip3 install --upgrade Cython==0.29.33 virtualenv
```

These might be needed as well, note that this is `python3.12-venv` (instead of `python3-virtualenv`):
```
sudo apt install build-essential git unp python3-pip python3-setuptools python3.12-venv
```

create the venv and install buildozer inside:
```
python -m venv venv
source venv/bin/activate
pip install setuptools
pip install buildozer
```

build the apk:
```
buildozer android debug
```

The build apk can be found in `./bin/`.

## Pipeline

adapted from
https://gist.github.com/zl475505/25245e8d28b13b3273e8bae1a63c4af2?permalink_comment_id=5144263#gistcomment-5144263