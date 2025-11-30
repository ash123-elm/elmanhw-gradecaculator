import os
import matplotlib.pyplot as plt

DATA_DIR = "data"
STUDENTS_FILE = os.path.join(DATA_DIR, "students.txt")
ASSIGNMENTS_FILE = os.path.join(DATA_DIR, "assignments.txt")
SUBMISSIONS_DIR = os.path.join(DATA_DIR, "submissions")

# ------------------------------------------------------------
# LOAD STUDENTS
# ------------------------------------------------------------
def load_students():
    students = {}      # name → id
    id_to_name = {}    # id → name

    with open(STUDENTS_FILE, "r") as f:
        for line in f:
            line = line.strip()
            if not line:
                continue

            sid = line[:3]                 # first 3 digits = student ID
            name = line[3:].strip()       # remainder = full name

            students[name] = sid
            id_to_name[sid] = name

    return students, id_to_name


# ------------------------------------------------------------
# LOAD ASSIGNMENTS (3-line format)
# ------------------------------------------------------------
def load_assignments():
    assignments = {}     # name → (id, points)
    id_to_name = {}      # id → name

    with open(ASSIGNMENTS_FILE, "r") as f:
        # Remove empty lines
        lines = [line.strip() for line in f if line.strip()]

    # Process 3 lines at a time
    for i in range(0, len(lines), 3):
        name = lines[i]
        aid = lines[i + 1]
        points = int(lines[i + 2])

        assignments[name] = (aid, points)
        id_to_name[aid] = name

    return assignments, id_to_name


# ------------------------------------------------------------
# LOAD SUBMISSIONS (format: SID|AID|PERCENT)
# ------------------------------------------------------------
def load_submissions():
    submissions = {}  # (student_id, assignment_id) → percent

    for file in os.listdir(SUBMISSIONS_DIR):
        path = os.path.join(SUBMISSIONS_DIR, file)
        if not file.endswith(".txt"):
            continue

        with open(path, "r") as f:
            for line in f:
                line = line.strip()
                if not line:
                    continue

                # Format: 132|68047|76
                parts = line.split("|")
                if len(parts) != 3:
                    continue

                sid, aid, percent = parts
                submissions[(sid, aid)] = float(percent)

    return submissions


# ------------------------------------------------------------
# OPTION 1: STUDENT GRADE
# ------------------------------------------------------------
def student_grade(name, students, assignments, submissions):
    if name not in students:
        print("Student not found")
        return

    sid = students[name]
    total_points = 0
    earned_points = 0

    for aname, (aid, points) in assignments.items():
        percent = submissions.get((sid, aid))
        if percent is None:
            continue  # this shouldn't happen per the rules

        total_points += points
        earned_points += (percent / 100) * points

    grade = round((earned_points / total_points) * 100)
    print(f"{grade}%")


# ------------------------------------------------------------
# OPTION 2: ASSIGNMENT STATS
# ------------------------------------------------------------
def assignment_stats(aname, assignments, submissions):
    if aname not in assignments:
        print("Assignment not found")
        return

    aid, points = assignments[aname]

    scores = []
    for (sid, a), percent in submissions.items():
        if a == aid:
            scores.append(percent)

    if not scores:
        print("Assignment not found")
        return

    print(f"Min: {int(min(scores))}%")
    print(f"Avg: {int(sum(scores)/len(scores))}%")
    print(f"Max: {int(max(scores))}%")


# ------------------------------------------------------------
# OPTION 3: HISTOGRAM
# ------------------------------------------------------------
def assignment_graph(aname, assignments, submissions):
    if aname not in assignments:
        print("Assignment not found")
        return

    aid, points = assignments[aname]

    scores = []
    for (sid, a), percent in submissions.items():
        if a == aid:
            scores.append(percent)

    if not scores:
        print("Assignment not found")
        return

    plt.hist(scores, bins=[0, 25, 50, 75, 100])
    plt.title(aname)
    plt.xlabel("Percent Score")
    plt.ylabel("Number of Students")
    plt.show()


# ------------------------------------------------------------
# MAIN MENU
# ------------------------------------------------------------
def main():
    students, id_to_name = load_students()
    assignments, id_to_name_assign = load_assignments()
    submissions = load_submissions()

    print("1. Student grade")
    print("2. Assignment statistics")
    print("3. Assignment graph\n")

    choice = input("Enter your selection: ").strip()

    if choice == "1":
        name = input("What is the student's name: ").strip()
        student_grade(name, students, assignments, submissions)

    elif choice == "2":
        aname = input("What is the assignment name: ").strip()
        assignment_stats(aname, assignments, submissions)

    elif choice == "3":
        aname = input("What is the assignment name: ").strip()
        assignment_graph(aname, assignments, submissions)

    else:
        print("Invalid selection.")


if __name__ == "__main__":
    main()
