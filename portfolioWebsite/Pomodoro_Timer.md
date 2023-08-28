# Pomodoro Timer

A simple GUI pomodoro timer implemented using Python Tkinter module.

---

This project implements a simple Pomodoro timer application using the Tkinter library in Python. The Pomodoro technique is a time management method that involves breaking work into intervals, typically 25 minutes of focused work (known as 'Pomodoros') followed by a 5 mins short break. After completing four consecutive Pomodoros, a longer break of 15 minutes is given as a reward.
1. Several constants and global variables such as colors, font, time intervals, and symbols are defined.
2. The *update_check_after_session()* function updates the checkmark label to indicate completed sessions.
3. The *reset_timer()* function cancels the ongoing timer and resets the timer display, repetition count, and other labels.
4. The *start_timer()* function initiates the Pomodoro timer mechanism by incrementing the repetition count, determining the session type (work or break), and starting the countdown.
5. The *count_down(count)* function handles the countdown mechanism by displaying the remaining time in minutes and seconds format. It recursively updates the timer display and triggers the start of a new session or update of completed sessions after a session ends.
6. The code sets up the user interface (UI) using Tkinter:
   - Labels to display the timer title, the timer itself (displayed as a tomato image and text), and a label for completed sessions.
   - Buttons for starting and resetting the timer.
   - The main UI window is configured with a yellow background and launched using *window.mainloop()*.

## Final Outcome
For illustrative purpose, I have set the *WORK_MIN*, *SHORT_BREAK_MIN* and *LONG_BREAK_MIN* to 1 minute.
<img width="555" alt="Pomodoro_Timer" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/a25e1eb7-a881-4f67-983f-8b210294c2d2">

## Project Code

```PYTHON
from tkinter import *

# ----------------------- CONSTANTS  & GLOBAL VARs------------------------------- #
PINK = "#e2979c"
RED = "#e7305b"
GREEN = "#9bdeac"
YELLOW = "#f7f5dd"
FONT_NAME = "Courier"
WORK_MIN = 25
SHORT_BREAK_MIN = 5
LONG_BREAK_MIN = 15
CHECK_EMOJI = "✔"
reps = 0
timer = None


def update_check_after_session():
    if reps > 1 and reps % 2 == 1:
        check_label.config(text=(reps // 2) * CHECK_EMOJI)


# ---------------------------- TIMER RESET ------------------------------- #
def reset_timer():
    window.after_cancel(timer)

    global reps
    reps = 0
    title_label.config(text="Timer", fg=GREEN)
    check_label.config(text=(reps // 2) * CHECK_EMOJI)
    canvas.itemconfig(timer_text, text="00:00")


# ---------------------------- TIMER MECHANISM ------------------------------- #
def start_timer():
    global reps
    reps += 1

    count_sec = 0
    if reps % 8 == 0:
        title_label.config(text="Break", fg=RED)
        count_sec = LONG_BREAK_MIN * 60
    elif reps % 2 == 1:
        title_label.config(text="Work", fg=GREEN)
        count_sec = WORK_MIN * 60
    else:
        title_label.config(text="Break", fg=PINK)
        count_sec = SHORT_BREAK_MIN * 60

    count_down(count_sec)


# ---------------------------- COUNTDOWN MECHANISM ------------------------------- #
def count_down(count):
    # format display into mm:ss
    secs = count % 60
    mins = count // 60

    canvas.itemconfig(timer_text, text=f"{mins:02d}:{secs:02d}")
    if count > 0:
        global timer
        timer = window.after(1000, count_down, count - 1)
    else:
        start_timer()
        update_check_after_session()


# ---------------------------- UI SETUP ------------------------------- #
window = Tk()
window.title("Pomodoro timer")
window.config(bg=YELLOW, padx=80, pady=50)

title_label = Label(text="Timer", fg=GREEN, bg=YELLOW, font=(FONT_NAME, 45, "bold"))
title_label.grid(row=0, column=1)

canvas = Canvas(width=200, height=224, bg=YELLOW, highlightthickness=0)
tomato_img = PhotoImage(file="tomato.png")
canvas.create_image(100, 112, image=tomato_img)
timer_text = canvas.create_text(
    100, 130, text="00:00", font=(FONT_NAME, 35, "bold"), fill="white"
)
canvas.grid(row=1, column=1)

start_button = Button(
    text="Start", bg=YELLOW, highlightbackground=YELLOW, command=start_timer
)
start_button.grid(row=2, column=0)

reset_button = Button(
    text="Reset", bg=YELLOW, highlightbackground=YELLOW, command=reset_timer
)
reset_button.grid(row=2, column=2)

check_label = Label(fg=GREEN, bg=YELLOW, font=(FONT_NAME, 30, "bold"))
check_label.grid(row=3, column=1)

window.mainloop()

```
