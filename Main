import tkinter as tk
from tkinter import filedialog, messagebox
import os
import shutil
import json
import threading
import time

class LabelEntryGenerator:
    def __init__(self):
        self.label = []
        self.entry = []

    def add_label(self, frame, label_text, font_style, font_size, bg_color, side):
        label = tk.Label(frame, text=label_text, font=(font_style, font_size), bg=bg_color)
        label.pack(side=side)
        self.label.append(label)
        return label
    
    def add_label_with_grid(self, frame, label_text, font_style, font_size, bg_color, image, row, column, padx, pady):
        label = tk.Label(frame, text=label_text, font=(font_style, font_size), bg=bg_color, anchor='center', image=image)
        label.grid(row=row, column=column, padx=padx, pady=pady, sticky=tk.W)
        self.label.append(label)
        return label
        
    def add_entry(self, frame, side, anchor, padx, show):
        entry = tk.Entry(frame, show=show, width=15, font=("Arial 15"))
        entry.pack(side=side, anchor=anchor, padx=padx)
        self.entry.append(entry)
        return entry
    
    def add_entry_with_grid(self, frame, row, column, padx, pady, show):
        entry = tk.Entry(frame, show=show, width=50, font=("Arial 15"))
        entry.grid(row=row, column=column, padx=padx, pady=pady, sticky=tk.W)
        self.entry.append(entry)
        return entry
        
    def destroy_label_entry(self):
        for label in self.label:
            label.pack_forget()
        for entry in self.entry:
            entry.pack_forget()

class File_Organizer(tk.Tk):
    def __init__(self):
        super().__init__()
        self.__configure_window()
        self.__label_entry = LabelEntryGenerator()
        self.__button = {}
        self.__directory = {}
        self.__organize_thread = None
        self.__is_running = False
        self.directory_frame = tk.Frame(self, bg='#AFDDE5')
        self.menu_frame = tk.Frame(self, bg='#002379', width=248, height=500)
        self.home_frame = tk.Frame(self, bg='#AFDDE5')
        self.help_frame = tk.Frame(self, bg='#AFDDE5')
        self.about_frame = tk.Frame(self, bg='#AFDDE5')  # New frame for the About section

    def __configure_window(self):
        self.title("File Organizer")
        self.configure(bg='#FFFFFF')
        self.wm_resizable(False, False)
    
    def home(self):
        self.directory_frame.grid_forget()
        self.menu_frame.grid_forget()
        self.home_frame.grid()
        title = self.__label_entry.add_label_with_grid(self.home_frame, "FILE ORGANIZER", "Open Sans Condensed", 50, '#AFDDE5', None, 0, 2, 10, 5)
        self.directory_btn = tk.Button(self.home_frame, text='Start', command=self.main_window, font=("Times New Roman", 15), height=1, width=10, bg='#AFDDE5')
        self.directory_btn.grid(row=1, column=2, padx=10, pady=5)

    def help(self):
        self.directory_frame.grid_forget()
        self.menu_frame.grid_forget()
        self.help_frame.grid(row=0, column=1)

        title = self.__label_entry.add_label_with_grid(self.help_frame, "HELP", "Open Sans Condensed", 50, '#AFDDE5', None, 0, 1, 10, 5)

        help_text = (
            "Welcome to the File Organizer Help Section!\n\n"
            "Instructions:\n"
            "1. Home: Click 'Home' to return to the main screen.\n"
            "2. Start: To begin organizing files, click 'Start' and select a directory and a configuration file.\n"
            "3. Browse Directory: Click 'Browse' to choose the directory containing files you want to organize.\n"
            "4. Browse Config: Click 'Browse' to select the configuration file (JSON) that defines how files should be organized.\n"
            "5. Start Organizing: Click 'Start Organize Files' to begin the file organization process based on the selected config file.\n"
            "6. Stop Organizing: Click 'Stop Organize Files' to halt the file organization process at any time.\n\n"
            "If you encounter any issues, please refer to the user manual or contact support for assistance."
        )

        help_text_label = tk.Label(self.help_frame, text=help_text, font=('Open Sans Condensed', 15), bg='#AFDDE5', justify='left')
        help_text_label.grid(row=1, column=1, padx=10, pady=5)

        self.back_btn = tk.Button(self.help_frame, text='Back', command=self.main_window, font=("Times New Roman", 15), height=1, width=10, bg='#AFDDE5')
        self.back_btn.grid(row=2, column=1, padx=10, pady=5)

    def about(self):
        self.directory_frame.grid_forget()
        self.menu_frame.grid_forget()
        self.about_frame.grid(row=0, column=1)

        title = self.__label_entry.add_label_with_grid(self.about_frame, "ABOUT", "Open Sans Condensed", 50, '#AFDDE5', None, 0, 1, 10, 5)

        about_text = (
            "File Organizer v1.0\n\n"
            "Developed by: Diether Pano and Jet Padilla\n"
            "Email: dietherpano12345@gmail.com\n"
            "Email: jetpadilla07@gmail.com\n"
            "GitHub: https://github.com/Nahiwagaan\n"
            "Github: https://github.com/DietherPano\n\n"
            "This application helps you organize files in a directory based on their types.\n"
            "Select a directory and a configuration file to start organizing your files.\n"
            "For more details, refer to the Help section."
        )

        about_text_label = tk.Label(self.about_frame, text=about_text, font=('Open Sans Condensed', 15), bg='#AFDDE5', justify='left')
        about_text_label.grid(row=1, column=1, padx=10, pady=5)

        self.back_btn = tk.Button(self.about_frame, text='Back', command=self.main_window, font=("Times New Roman", 15), height=1, width=10, bg='#AFDDE5')
        self.back_btn.grid(row=2, column=1, padx=10, pady=5)

    def main_window(self):
        self.home_frame.grid_forget()
        self.help_frame.grid_forget()
        self.about_frame.grid_forget()
        self.directory_frame.grid(row=0, column=1)
        directory_label = self.__label_entry.add_label_with_grid(self.directory_frame, "Select Directory", "Open Sans Condensed", 15, '#AFDDE5', None, 0, 0, 10, 5)
        self.__directory['Directory'] = self.__label_entry.add_entry_with_grid(self.directory_frame, 0, 1, 10, 5, '')
        self.directory_btn = tk.Button(self.directory_frame, text='Browse', command=self.browse_directory, font=("Times New Roman", 15), height=1, width=10, bg='#AFDDE5')
        self.directory_btn.grid(row=0, column=2, padx=10, pady=5)
        
        config_label = self.__label_entry.add_label_with_grid(self.directory_frame, "Select Config File", "Open Sans Condensed", 15, '#AFDDE5', None, 1, 0, 10, 5)
        self.__directory['Config'] = self.__label_entry.add_entry_with_grid(self.directory_frame, 1, 1, 10, 5, '')
        self.config_btn = tk.Button(self.directory_frame, text='Browse', command=self.browse_config_file, font=("Times New Roman", 15), height=1, width=10, bg='#AFDDE5')
        self.config_btn.grid(row=1, column=2, padx=10, pady=5)

        self.menu_frame.grid(row=0, column=0)
        self.home_btn = tk.Button(self.menu_frame, text='Home', command=self.home, font=("Times New Roman", 15), height=1, width=10, bg='#AFDDE5')
        self.home_btn.grid(row=1, column=0, padx=10, pady=5)
        self.help_btn = tk.Button(self.menu_frame, text='Help', command=self.help, font=("Times New Roman", 15), height=1, width=10, bg='#AFDDE5')
        self.help_btn.grid(row=2, column=0, padx=10, pady=5)
        self.about_btn = tk.Button(self.menu_frame, text='About', command=self.about, font=("Times New Roman", 15), height=1, width=10, bg='#AFDDE5')
        self.about_btn.grid(row=3, column=0, padx=10, pady=5)

        self.__button['Start_btn'] = tk.Button(self.directory_frame, text='Start Organize Files', command=self.start_organize_files, font=("Times New Roman", 15), height=1, bg='#AFDDE5')
        self.__button['Start_btn'].grid(row=2, column=0, padx=10, pady=5)

        self.__button['Stop_btn'] = tk.Button(self.directory_frame, text='Stop Organize Files', command=self.stop_organize_files, font=("Times New Roman", 15), height=1, bg='#AFDDE5', state=tk.DISABLED)
        self.__button['Stop_btn'].grid(row=2, column=1, padx=10, pady=5)

    def browse_directory(self):
        directory_path = filedialog.askdirectory()
        if directory_path:
            self.__directory['Directory'].delete(0, tk.END)
            self.__directory['Directory'].insert(0, directory_path)

    def browse_config_file(self):
        file_path = filedialog.askopenfilename(filetypes=[('JSON file', '.json')])
        if file_path:
            self.__directory['Config'].delete(0, tk.END)
            self.__directory['Config'].insert(0, file_path)

    def start_organize_files(self):
        directory = self.__directory['Directory'].get()
        config_file = self.__directory['Config'].get()

        if not os.path.exists(directory):
            messagebox.showerror('Error', f'Directory "{directory}" does not exist')
            return
        
        if not os.path.isfile(config_file):
            messagebox.showerror("Error", f"Invalid configuration file path: '{config_file}'")
            return
        
        if not self.__is_running:
            self.__is_running = True
            self.__button['Start_btn'].config(state=tk.DISABLED)
            self.__button['Stop_btn'].config(state=tk.NORMAL)
            self.__organize_thread = threading.Thread(target=self.run_continuously, args=(directory, config_file))
            self.__organize_thread.start()
            # messagebox.showinfo("Sorting Started", "Automatic sorting has started")
    
    def stop_organize_files(self):
        if self.__is_running:
            self.__is_running = False
            self.__button['Start_btn'].config(state=tk.NORMAL)
            self.__button['Stop_btn'].config(state=tk.DISABLED)
            # messagebox.showinfo("Sorting Stopped", "Automatic sorting has stopped")

    def load_file_type_json(self, json_path):
        with open(json_path, 'r') as file:
            return json.load(file)
    
    def create_directory_if_does_not_exist(self, directory):
        if not os.path.exists(directory):
            os.makedirs(directory)
    
    def move_file_to_directory(self, src_file, destination_directory):
        shutil.move(src_file, destination_directory)

    def selection_sort(self, array_files):
        n = len(array_files)
        for i in range(n - 1):
            min_index = i
            for j in range(i + 1, n):
                file1 = array_files[j]
                file2 = array_files[min_index]
                extension1 = os.path.splitext(file1)[1].lower()
                extension2 = os.path.splitext(file2)[1].lower()

                if extension1 < extension2:
                    min_index = j
            if min_index != i:
                array_files[i], array_files[min_index] = array_files[min_index], array_files[i]
        return array_files

    def organize_files(self, directory, config):
        file_types = self.load_file_type_json(config)
        files_to_sort = []

        for filename in os.listdir(directory):
            src_file = os.path.join(directory, filename)
            if os.path.isfile(src_file):
                files_to_sort.append(filename)

        sorted_files = self.selection_sort(files_to_sort)

        for filename in sorted_files:
            src_file = os.path.join(directory, filename)
            file_extension = os.path.splitext(filename)[1].lower()

            moved = False

            for file_type_dictionary in [file_types['file_types'], file_types['code_file_types'], file_types['database_file_types']]:
                for file_type, extensions in file_type_dictionary.items():
                    if file_extension in extensions:
                        destination_directory = os.path.join(directory, file_type)
                        self.create_directory_if_does_not_exist(destination_directory)

                        dest_file_path = os.path.join(destination_directory, os.path.basename(src_file))
                        # Add (1) to File if there is a duplicate
                        if os.path.exists(dest_file_path):
                            count = 1
                            base, ext = os.path.splitext(os.path.basename(src_file))
                            while os.path.exists(dest_file_path):
                                dest_file_path = os.path.join(destination_directory, f'{base} ({count}){ext}')
                                count += 1
                        self.move_file_to_directory(src_file, dest_file_path)
                        moved = True
                        break
                if moved:
                    break

            if not moved:
                destination_directory = os.path.join(directory, "Miscellaneous")
                self.create_directory_if_does_not_exist(destination_directory)
                dest_file_path = os.path.join(destination_directory, os.path.basename(src_file))
                # Add (1) to File if there is a duplicate
                if os.path.exists(dest_file_path):
                    count = 1
                    base, ext = os.path.splitext(os.path.basename(src_file))
                    while os.path.exists(dest_file_path):
                        dest_file_path = os.path.join(destination_directory, f'{base} ({count}){ext}')
                        count += 1
                self.move_file_to_directory(src_file, dest_file_path)

    def run_continuously(self, directory, config):
        while self.__is_running:
            try:
                self.organize_files(directory, config)
                self.update_idletasks()
                time.sleep(1)  # Adjust the interval (in seconds) for sorting
            except Exception as e:
                print(f"Error occurred during sorting: {str(e)}")
                messagebox.showerror("Sorting Error", f"An error occurred during sorting: {str(e)}")
                self.__is_running = False
                self.__button['Start_btn'].config(state=tk.NORMAL)
                self.__button['Stop_btn'].config(state=tk.DISABLED)

    def exit(self):
        self.destroy()

    def display(self):
        self.home()
        self.mainloop()

if __name__ == '__main__':
    app = File_Organizer()
    app.display()
