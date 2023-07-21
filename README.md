# Pomodoro Timer

![pomodoro](https://github.com/buczyniak/Pomodoro-timer/assets/78871310/7428aceb-39e6-4db5-9d7d-80f33defd67e)

## Introduction - What a Pomodoro Timer is
A Pomodoro Timer is a time management technique designed to improve productivity and focus. It was developed by Francesco Cirillo in the late 
1980s. The technique involves breaking work into intervals called "Pomodoros," typically 25 minutes long, followed by short breaks. After 
completing a set of Pomodoros, a longer break is taken. The timer helps users maintain a balance between focused work and regular breaks, 
promoting better concentration and avoiding burnout.

The Timer was created while taking a [Udemy course](https://www.udemy.com/course/100-days-of-code/)

## Explanation of the Code
My Python code is an implementation of a graphical Pomodoro Timer using the Tkinter library. Let's go through the main components of the code.

### Constants:
```
# ---------------------------- CONSTANTS ------------------------------- #
PINK = "#e2979c"
RED = "#e7305b"
GREEN = "#9bdeac"
YELLOW = "#f7f5dd"
FONT_NAME = "Courier"
WORK_MIN = 25
SHORT_BREAK_MIN = 5
LONG_BREAK_MIN = 20
reps = 0
timer = None
```
* The code defines some color constants (PINK, RED, GREEN, YELLOW) and font name (FONT_NAME) used in the user interface.
* The code also sets the durations for work, short breaks, and long breaks (WORK_MIN, SHORT_BREAK_MIN, LONG_BREAK_MIN) in minutes.

Two variables are initialized:
* reps: Keeps track of the number of Pomodoros and breaks completed.
* timer: Holds the reference to the timer event used for counting down.

### Timer Reset Function:
![pomodoro_reset](https://github.com/buczyniak/Pomodoro-timer/assets/78871310/429f9740-9c40-4c52-a39e-3137db4b1cd2)
```
# ---------------------------- TIMER RESET ------------------------------- #

def reset_timer():
    window.after_cancel(timer)
    #timer_text 00:00
    canvas.itemconfig(timer_text, text="00:00")
    #title_label "Timer"
    title.config(text="Timer")
    #reset check_marks
    check_marks.config(text="")
    global reps
    reps = 0
```

The reset_timer() function is responsible for resetting the timer to its initial state. When called, it cancels any ongoing timer event, sets 
the displayed time to "00:00", resets the title to "Timer," and clears any checkmarks displayed.

### Timer Mechanism:
```
# ---------------------------- TIMER MECHANISM ------------------------------- #
def start_timer():
    global reps
    reps += 1

    work_sec = WORK_MIN * 60
    short_break_sec = SHORT_BREAK_MIN * 60
    long_break_sec = LONG_BREAK_MIN * 60

    if reps % 8 == 0:
        countdown(long_break_sec)
        title.config(text="Break", fg=RED)

    elif reps % 2 == 0:
        countdown(short_break_sec)
        title.config(text="Break", fg=PINK)

    else:
        countdown(work_sec)
        title.config(text="Work", fg=GREEN)
```
The start_timer() function handles the logic of starting the timer for either work or break sessions. It updates the title of the timer and 
calls the countdown() function with the appropriate duration based on the number of completed Pomodoros.

### Countdown Mechanism:
```
# ---------------------------- COUNTDOWN MECHANISM ------------------------------- #
def countdown(count):
    count_min = math.floor(count / 60)
    count_sec = count % 60

    if count_sec < 10:
        count_sec = f"0{count_sec}"

    canvas.itemconfig(timer_text, text=f"{count_min}:{count_sec}")
    if count > 0:
        global timer
        timer = window.after(1000, countdown, count - 1)
    else:
        start_timer()
        marks = ""
        work_sessions = math.floor(reps / 2)
        for _ in range(work_sessions):
            marks += "✔"
        check_marks.config(text=marks)
```
The countdown() function takes a count argument, representing the time in seconds, and displays the remaining time in minutes and seconds format. 
It also updates the timer display every second until the countdown reaches 0. When the countdown finishes, it calls the start_timer() function 
again for the next session and adds checkmarks based on the number of completed work sessions.

### User Interface Setup:
```
# ---------------------------- UI SETUP ------------------------------- #
window = Tk()
window.title("Pomodoro")
window.config(padx=100, pady=50, bg=YELLOW)

canvas = Canvas(width=205, height=224, bg=YELLOW, highlightthickness=0)
tomato_img = PhotoImage(file="tomato.png")
canvas.create_image(102, 112, image=tomato_img)
timer_text = canvas.create_text(103, 130, text="00:00", fill="white", font=(FONT_NAME, 35, "bold"))
canvas.grid(column=1, row=1)

title = Label(text="Timer", font=(FONT_NAME, 35, "bold"), fg=GREEN)
title.config(bg=YELLOW)
title.grid(column=1, row=0)

button_start = Button(text="Start", command=start_timer)
button_start.grid(column=0, row=2)

button_reset = Button(text="Reset", command=reset_timer)
button_reset.grid(column=2, row=2)

check_marks = Label(fg=GREEN, bg=YELLOW)
check_marks.grid(column=1, row=3)

window.mainloop()
```
The code creates a graphical user interface (GUI) using Tkinter. The GUI consists of:

* A window titled "Pomodoro" with a yellow background (bg=YELLOW).
* A canvas that displays a tomato image and the timer text in the center.
* A label (title) displaying "Timer" at the top center with a green color.
* A "Start" button (button_start) to begin the timer.
* A "Reset" button (button_reset) to reset the timer.
* A label (check_marks) to display checkmarks for completed work sessions.

## Summary
Overall, this Pomodoro Timer GUI provides a user-friendly way to implement the Pomodoro technique, helping users stay productive and focused 
while taking regular breaks. The timer switches between work and break sessions automatically and keeps track of completed Pomodoros with checkmarks.
