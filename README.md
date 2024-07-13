import tkinter as tk
from tkinter import messagebox
import requests
import threading

# Function to send a request to a URL
def send_request(url):
    try:
        response = requests.get(url)
        print(f"Status Code: {response.status_code}")
    except requests.RequestException as e:
        print(f"Request failed: {e}")

# Function to perform a stress test
def stress_test(url, num_requests):
    threads = []
    for _ in range(num_requests):
        thread = threading.Thread(target=send_request, args=(url,))
        thread.start()
        threads.append(thread)

    for thread in threads:
        thread.join()

# Function to start the stress test
def start_test():
    url = entry_url.get()
    try:
        num_requests = int(entry_requests.get())
        stress_test(url, num_requests)
    except ValueError:
        messagebox.showerror("Invalid input", "Please enter a valid number of requests.")

# Create the main window
root = tk.Tk()
root.title("Webpage Stress Test")

# Create and place the URL label and entry
tk.Label(root, text="URL:").grid(row=0, column=0, padx=10, pady=10)
entry_url = tk.Entry(root, width=50)
entry_url.grid(row=0, column=1, padx=10, pady=10)

# Create and place the Number of Requests label and entry
tk.Label(root, text="Number of Requests:").grid(row=1, column=0, padx=10, pady=10)
entry_requests = tk.Entry(root, width=50)
entry_requests.grid(row=1, column=1, padx=10, pady=10)

# Create and place the Start Test button
btn_start = tk.Button(root, text="Start Test", command=start_test)
btn_start.grid(row=2, column=0, columnspan=2, pady=20)

# Run the application
root.mainloop()
