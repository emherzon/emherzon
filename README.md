import customtkinter as ctk
import keyboard  # Module for capturing global key presses
from PIL import Image  # To handle images with customtkinter

# Class to handle each individual timer
class Timer:
    def __init__(self, image, name, duration, label):
    def __init__(self, image, name, duration, label, loop=False):
        self.image = image
        self.name = name
        self.duration = duration
@@ -13,6 +13,7 @@
        self.running = False
        self.paused = False  # State to check if the timer is paused
        self.timer_id = None  # To store the after() callback ID
        self.loop = loop  # New loop attribute

    # Start the timer using the `after()` method
    def start(self):
@@ -30,13 +31,17 @@
            self.update_label(f"{self.name} Time's up!", "#5fce4e")
            self.running = False  # Stop the timer when time's up

            # If looping is enabled, restart the timer
            if self.loop and self.remaining_time != -3:
                self.reset()  # Reset will also restart the countdown
    # Update the label text and color
    def update_label(self, text, color):
        self.label.configure(text=text, text_color=color)

    # Reset the timer to its initial state
    def reset(self):
        if self.name not in ["Freed Shadow", "AOSM", "Night Parade", "GROTTO"] or self.remaining_time <= 0:
        if self.name not in ["Freed Shadow", "AOSM", "Night Parade", "GROTTO", "T-Slot Skills"] or self.remaining_time <= 0:
            if self.timer_id:
                self.label.after_cancel(self.timer_id)
            self.remaining_time = self.duration
@@ -77,9 +82,9 @@
                timer.pause()
            update_status_label(status_label, "Paused", "#ff9933")  # Orange color for "Paused"
            all_paused = True
    elif key == 'f3':  # Change 'F2' to what you want to reset timers 
    elif key == 'f3':  # Change 'F3' to what you want to reset timers
        for name, timer in timers.items():
            timer.remaining_time = 0
            timer.remaining_time = -3
    else:  # Handle other keys for resetting individual timers
        for name, timer in timers.items():
            # Check if the key combination is pressed
@@ -102,7 +107,10 @@

    # Configuration of initial timer values and key bindings
    timer_settings = {
    "Example Name": {"duration": 15, "key": "r"},  # New timer added   
        "Example Name": {"duration": 15, "key": "r"},  # New timer    
        "Example Name": {"duration": 15, "key": "r", "image": "xxx.png"},  # New timer with image
        "Example Name": {"duration": 15, "key": "r", "loop": True},  # New timer with loop
        "Example Name": {"duration": 15, "key": "r", "image": "xxx.png", "loop": True},  # New timer with loop and image
    }

    # Create the main application window
@@ -127,18 +135,19 @@

        # Check if 'image' key exists and load image accordingly
        if 'image' in config:
            if name in ["Freed Shadow", "AOSM", "Night Parade", "GROTTO"]:
            if name in ["Freed Shadow", "AOSM", "Night Parade", "GROTTO", "PT1", "PT2", "PT3", "PT4"]:
                ctkImage = ctk.CTkImage(light_image=Image.open(config['image']), dark_image=Image.open(config['image']), size=(65, 50))
            else:
                ctkImage = ctk.CTkImage(light_image=Image.open(config['image']), dark_image=Image.open(config['image']), size=(50, 50))
        else:
            ctkImage = None  # No image specified

        label = ctk.CTkLabel(app, image=ctkImage,compound="left", padx=10, text=f"{name} Time Left: 0 sec", font=("Helvetica", 18))
        label = ctk.CTkLabel(app, image=ctkImage, compound="left", padx=10, text=f"{name} Time Left: 0 sec", font=("Helvetica", 18))
        label.pack(pady=10)

        # Create and store Timer object
        timer = Timer(config.get("image"), name, config["duration"], label)
        loop = config.get("loop", False)  # Retrieve the loop setting, default to False if not provided
        timer = Timer(config.get("image"), name, config["duration"], label, loop=loop)
        timer.key = config["key"]  # Store key in timer object for reference
        timers[name] = timer
