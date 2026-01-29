import tkinter as tk
from tkinter import filedialog, messagebox
import pandas as pd
import os
import zipfile

def convert_and_zip(parquet_paths):
    output_dir = filedialog.askdirectory(title="Select Output Folder")
    if not output_dir:
        return

    csv_files = []

    for file in parquet_paths:
        try:
            df = pd.read_parquet(file)
            csv_name = os.path.splitext(os.path.basename(file))[0] + ".csv"
            csv_path = os.path.join(output_dir, csv_name)
            df.to_csv(csv_path, index=False)
            csv_files.append(csv_path)
            log(f"✅ Converted: {csv_name}")
        except Exception as e:
            log(f"❌ Error converting {file}: {e}")

    if zip_var.get():
        zip_path = os.path.join(output_dir, "converted_csvs.zip")
        with zipfile.ZipFile(zip_path, 'w') as zipf:
            for csv in csv_files:
                zipf.write(csv, arcname=os.path.basename(csv))
        log(f"🗜️ Zipped CSVs into: {zip_path}")

def select_files():
    files = filedialog.askopenfilenames(title="Select Parquet Files", filetypes=[("Parquet Files", "*.parquet")])
    if files:
        file_list.delete(0, tk.END)
        for f in files:
            file_list.insert(tk.END, f)
        convert_button.config(state="normal")

def start_conversion():
    parquet_files = file_list.get(0, tk.END)
    if not parquet_files:
        messagebox.showwarning("No Files", "Please select at least one Parquet file.")
        return
    convert_and_zip(parquet_files)

def log(message):
    status_text.config(state=tk.NORMAL)
    status_text.insert(tk.END, message + "\n")
    status_text.see(tk.END)
    status_text.config(state=tk.DISABLED)

# GUI Setup
root = tk.Tk()
root.title("Parquet to CSV Converter")
root.geometry("600x400")

frame = tk.Frame(root)
frame.pack(pady=10)

tk.Button(frame, text="Select Parquet Files", command=select_files).pack()

file_list = tk.Listbox(root, width=80, height=6)
file_list.pack()

zip_var = tk.BooleanVar()
tk.Checkbutton(root, text="Zip output CSVs", variable=zip_var).pack(pady=5)

convert_button = tk.Button(root, text="Convert", command=start_conversion, state="disabled")
convert_button.pack(pady=10)

status_text = tk.Text(root, height=10, state=tk.DISABLED)
status_text.pack(padx=10, pady=10, fill=tk.BOTH, expand=True)

root.mainloop()
