import tkinter as tk
import random
import time
from tkinter import ttk

def bubble_sort(data, draw_callback, delay):
    n = len(data)
    for i in range(n):
        already_sorted = True
        for j in range(n - i - 1):
            if data[j] > data[j + 1]:
                data[j], data[j + 1] = data[j + 1], data[j]
                already_sorted = False
                draw_callback(data, ['red' if x == j or x == j+1 else 'blue' for x in range(len(data))])
                time.sleep(delay)
        if already_sorted:
            break

def selection_sort(data, draw_callback, delay):
    n = len(data)
    for i in range(n):
        min_idx = i
        for j in range(i + 1, n):
            if data[j] < data[min_idx]:
                min_idx = j
            draw_callback(data, ['green' if x == j else 'blue' for x in range(len(data))])
            time.sleep(delay)
        data[i], data[min_idx] = data[min_idx], data[i]
        draw_callback(data, ['red' if x == i or x == min_idx else 'blue' for x in range(len(data))])
        time.sleep(delay)

def draw_data(data, color_array):
    canvas.delete("all")
    c_height = 300
    c_width = 600
    x_width = c_width / (len(data) + 1)
    spacing = 5
    normalized_data = [i / max(data) for i in data]

    for i, height in enumerate(normalized_data):
        x0 = i * x_width + spacing
        y0 = c_height - height * 260
        x1 = (i + 1) * x_width
        y1 = c_height
        canvas.create_rectangle(x0, y0, x1, y1, fill=color_array[i])
        canvas.create_text(x0 + 2, y0, anchor=tk.SW, text=str(data[i]), font=("Arial", 8))

    root.update_idletasks()

def generate():
    global data
    data = [random.randint(1, 100) for _ in range(30)]
    draw_data(data, ['blue' for _ in range(len(data))])

def start_sort():
    global data
    if algo_menu.get() == 'Bubble Sort':
        bubble_sort(data, draw_data, 0.05)
    elif algo_menu.get() == 'Selection Sort':
        selection_sort(data, draw_data, 0.05)

root = tk.Tk()
root.title("Visualizador de Algoritmos de Ordenamiento")

algo_menu = ttk.Combobox(root, values=['Bubble Sort', 'Selection Sort'])
algo_menu.grid(row=0, column=0, padx=10, pady=10)
algo_menu.current(0)

tk.Button(root, text="Generar", command=generate, bg="lightgray").grid(row=0, column=1, padx=10, pady=10)
tk.Button(root, text="Iniciar", command=start_sort, bg="lightgreen").grid(row=0, column=2, padx=10, pady=10)

canvas = tk.Canvas(root, width=600, height=300, bg="white")
canvas.grid(row=1, column=0, columnspan=3, padx=10, pady=10)

data = []

root.mainloop()
