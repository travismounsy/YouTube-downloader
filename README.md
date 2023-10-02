# YouTube-downloader
# YouTube downloader, using tkinter to develop a python GUI program to download videos in the highest resolution with a visual download progress bar. 

import tkinter as tk
from tkinter import filedialog


import customtkinter

from pytube import YouTube


# def files():
def download():
    try:
        ytLink = link.get()
        ytObject = YouTube(ytLink, on_progress_callback=on_progress)
        video = ytObject.streams.get_highest_resolution()
        title.configure(text=ytObject.title, text_color="black")
        finishLabel.configure("")
        video.download()
        finishLabel.configure(text="Downloaded!")
    except:
        finishLabel.configure(text="Download Error", text_color="red")


def download1():
    try:
        ytLink = link.get()
        ytObject = YouTube(ytLink, on_progress_callback=on_progress)
        audio = ytObject.streams.get_audio_only()
        title.configure(text=ytObject.title, text_color="white")
        finishLabel.configure("")
        audio.download()
        finishLabel.configure(text="Downloaded!")
    except:
        finishLabel.configure(text="Download Error", text_color="red")


def on_progress(stream, chunk, bytes_remaining):
    total_size = stream.filesize
    bytes_downloaded = total_size - bytes_remaining
    percentage_of_completion = bytes_downloaded / total_size * 100
    per = str(int(percentage_of_completion))
    pPercentage.configure(text=per + '%')
    pPercentage.update()

    progressBar.set(float(percentage_of_completion) / 100)


# system settings
customtkinter.set_appearance_mode("Light")
customtkinter.set_default_color_theme("blue")

# app frame
app = customtkinter.CTk()
app.geometry("720x480")
app.title("YouTube Downloader")

# adding UI elements
title = customtkinter.CTkLabel(app, text="Insert a YouTube link")
title.pack(padx=10, pady=10)

# link input
url_var = customtkinter.StringVar()
link = customtkinter.CTkEntry(app, width=350, height=40, textvariable=url_var)
link.pack()

# finishedDownloading
finishLabel = customtkinter.CTkLabel(app, text="")
finishLabel.pack(padx=10, pady=10)

# progress percentage
pPercentage = customtkinter.CTkLabel(app, text="0%")
pPercentage.pack()

# progress bar
progressBar = customtkinter.CTkProgressBar(app, width=400)
progressBar.set(0)
progressBar.pack(padx=10, pady=10)

# paste button
paste = customtkinter.CTkButton(app, text="Paste link", command="")
paste.pack(padx=10, pady=10)

# video download button
download = customtkinter.CTkButton(app, text="Download video", command=download)
download.pack(padx=10, pady=10)

# audio download button
download1 = customtkinter.CTkButton(app, text="Download audio", command=download1)
download1.pack(padx=10, pady=10)

# open file path location
file = customtkinter.CTkButton(app, text="Open file location", command="")
file.pack(padx=10, pady=10)

# run app
app.mainloop()

