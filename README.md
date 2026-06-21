# worksheet-8b-coding-tools-
CADT Digital Literacy Lab – CSV cybersecurity row counter
import csv
import os

def count_cybersecurity_rows(filename):
    """
    Reads a CSV file and counts the number of rows
    that contain the word 'cybersecurity' (case-insensitive).

    Parameters:
        filename (str): Path to the CSV file

    Returns:
        int: Number of rows containing 'cybersecurity'
    """

    # Check if file exists
    if not os.path.exists(filename):
        raise FileNotFoundError(f"File '{filename}' not found.")

    # Check if file is empty
    if os.path.getsize(filename) == 0:
        raise ValueError(f"File '{filename}' is empty.")

    # Check if file is a CSV
    if not filename.endswith('.csv'):
        raise TypeError(f"Expected a .csv file, got: '{filename}'")

    count = 0

    with open(filename, newline='', encoding='utf-8') as f:
        reader = csv.reader(f)
        for row in reader:
            for cell in row:
                if "cybersecurity" in cell.lower():
                    count += 1
                    break  # Count the row only once even if multiple cells match

    return count


# ── Test the function ──────────────────────────────────────────────────────────

if __name__ == "__main__":

    # Create a sample CSV for testing
    sample_data = """title,description
Intro to cybersecurity,Learn the basics of security
Python for Data Science,No security topics here
Advanced Cybersecurity,Ethical hacking and defense
Web Development,HTML and CSS fundamentals
Cybersecurity for Beginners,Start your security journey
"""
    with open("sample_data.csv", "w") as f:
        f.write(sample_data)

    # Normal case
    result = count_cybersecurity_rows("sample_data.csv")
    print(f"Rows containing 'cybersecurity': {result}")

    # Edge case 1 - Empty file
    with open("empty.csv", "w") as f:
        pass
    try:
        count_cybersecurity_rows("empty.csv")
    except ValueError as e:
        print(f"Empty file error caught: {e}")

    # Edge case 2 - File not found
    try:
        count_cybersecurity_rows("missing.csv")
    except FileNotFoundError as e:
        print(f"File not found error caught: {e}")

    # Edge case 3 - Wrong data type
    with open("notes.txt", "w") as f:
        f.write("cybersecurity notes")
    try:
        count_cybersecurity_rows("notes.txt")
    except TypeError as e:
        print(f"Wrong file type error caught: {e}")
