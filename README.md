# -File-Organizer-
import os
import shutil

# --------- File Organizer Tool ---------
# Organizes files in a given directory by file type

def organize_files(directory):
    if not os.path.isdir(directory):
        print("Invalid directory path.")
        return

    # File type categories
    file_types = {
        "Images": [".jpg", ".jpeg", ".png", ".gif", ".bmp"],
        "Documents": [".pdf", ".docx", ".doc", ".txt", ".pptx", ".xlsx"],
        "Videos": [".mp4", ".mkv", ".mov", ".avi"],
        "Music": [".mp3", ".wav", ".aac"],
    }

    # Create folders if not exist
    for folder in file_types.keys():
        folder_path = os.path.join(directory, folder)
        if not os.path.exists(folder_path):
            os.makedirs(folder_path)

    # Loop through each file
    for file in os.listdir(directory):
        file_path = os.path.join(directory, file)

        # Skip directories
        if os.path.isdir(file_path):
            continue

        file_ext = os.path.splitext(file)[1].lower()

        # Move file to matching category
        moved = False
        for folder, extensions in file_types.items():
            if file_ext in extensions:
                shutil.move(file_path, os.path.join(directory, folder, file))
                moved = True
                break

        # If no category matches, move to "Others"
        if not moved:
            other_folder = os.path.join(directory, "Others")
            if not os.path.exists(other_folder):
                os.makedirs(other_folder)
            shutil.move(file_path, os.path.join(other_folder, file))

    print("Files organized successfully!")

# --------- Run ---------
if __name__ == "__main__":
    path = input("Enter the folder path to organize: ")
    organize_files(path)

