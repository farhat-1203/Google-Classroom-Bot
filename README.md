<h1 align="center">Google Classroom Bot</h1>
<p align="center"><br>
This Python bot automates joining Google Classroom meetings based on a predefined schedule. It uses Selenium WebDriver to log in to Google Classroom, navigate to the correct class, and join Google Meet sessions at the scheduled time.<br></p>

## Requirements

- Clone this repository.
- [Python 3](https://www.python.org/downloads/).
- [Firefox browser](https://www.mozilla.org/en-US/firefox/all/#product-desktop-release).
- Geckodriver (to interface with Firefox; download from [here](https://github.com/mozilla/geckodriver/releases)).
- Install dependencies:
  ```bash
  pip install -r requirements.txt
  ```

## Setup

1. **Clone the Repository**:
   ```bash
   git clone <repository-link>
   cd GoogleClassroom-Bot
   ```

2. **Set Up Credentials**:
   - Open `config.ini` and add your Google account credentials:
     ```ini
     [AUTH]
     USERNAME=username@abc.com
     PASSWORD=password
     ```

3. **Download and Configure Geckodriver**:
   - Download Geckodriver from [here](https://github.com/mozilla/geckodriver/releases).
   - Place the Geckodriver executable in the project folder or ensure it's in your system's PATH.

4. **Set Up Firefox Profile**:
   - Create a new profile in Firefox (to block camera and microphone permissions for Google Meet):
     - Open `about:profiles` in Firefox.
     - Create a new profile and configure it to block permissions for the camera and microphone.
     - Note the path to the newly created profile.
   - Update the path in the `login` function of the script:
     ```python
     profile = webdriver.FirefoxProfile('/path/to/the/created/profile')
     ```

5. **Prepare the Schedule**:
   - Open `schedule.csv` and populate it with your class schedule.
   - Ensure the course names match the names in Google Classroom.
   - Example schedule format:
     | Day  | 09:20         | 11:40         | 14:25         |
     |------|---------------|---------------|---------------|
     | Mon  | CS16004-SemC  |               |               |
     | Tue  |               | CS16004-SemC  |               |
     | Thu  |               |               | CS16004-SemC  |

## Execution

1. Navigate to the project folder:
   ```bash
   cd GoogleClassroom-Bot
   ```

2. Run the script:
   ```bash
   python3 gmeet_bot.py
   ```

3. To stop execution, use `Ctrl+C` in the terminal.

## Features

- **Automated Class Joining**:
  - Logs into Google Classroom.
  - Finds the appropriate class based on the current schedule.
  - Extracts the Google Meet link and joins the meeting.
  - Ends the meeting after one hour and prepares for the next scheduled class.

- **Customizable Timings**:
  - Modify the `classTime` list in the script to include your class timings (24-hour format).

- **Error Handling**:
  - Skips execution if no classes are scheduled for the day.

## Troubleshooting

1. Ensure that the Google account isn't already logged in on the browser.
2. The class names in `schedule.csv` must exactly match those in Google Classroom.
3. Slow internet may cause Selenium to throw errors. Increase sleep intervals in the script if needed.

## Source Code Overview

### Key Components

1. **Configuration**:
   - The script reads credentials from `config.ini`.

2. **Scheduling**:
   - `schedule.csv` contains the timetable.
   - The script determines the next class based on the current day and time.

3. **Automation with Selenium**:
   - Firefox WebDriver is used to log in, navigate to classes, and join Google Meet sessions.

4. **Login Functionality**:
   - The `login()` function authenticates the user in Google Classroom using the provided credentials and Firefox profile.

5. **Class Joining**:
   - The bot checks the current schedule and joins the appropriate Google Meet session using the provided link.

### Code Breakdown

- **Main Class**:
  - `ClassAutomation` runs the entire process and repeats at 30-second intervals.

- **Methods**:
  - `initClass`: Initiates the process of joining a class.
  - `findClass`: Finds the current class based on the schedule and time.
  - `findCount`: Determines the position in the schedule for the next class.
  - `login`: Handles login to Google Classroom and navigation.

## Limitations

- Designed for three classes per day. To increase, modify the following in the script:
  ```python
  if self.count < 2:  # Change 2 to n-1 for n classes per day
      self.count = self.count + 1
  ```

- The script assumes that classes last exactly one hour. Adjust the `sleep(60 * 60)` in `initClass` for different durations.

## Future Improvements

- Add support for other browsers (e.g., Chrome, Edge).
- Enhance error handling for login and connection issues.
- Add a graphical user interface (GUI) for easier setup and schedule management.

