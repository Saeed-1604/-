#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <sstream>

using namespace std;

struct Student {
    string id;
    string name;
    string major;
    int year;
};

void saveStudentsToFile(const vector<Student>& students) {
    ofstream file("students.txt");
    for (const auto& s : students) {
        file << s.id << "," << s.name << "," << s.major << "," << s.year << "\n";
    }
    file.close();
}

void loadStudentsFromFile(vector<Student>& students) {
    students.clear();
    ifstream file("students.txt");
    string line;
    while (getline(file, line)) {
        stringstream ss(line);
        string id, name, major, yearStr;
        getline(ss, id, ',');
        getline(ss, name, ',');
        getline(ss, major, ',');
        getline(ss, yearStr);
        Student s = { id, name, major, stoi(yearStr) };
        students.push_back(s);
    }
    file.close();
}

void addStudent(vector<Student>& students) {
    Student s;
	cout << "Enter student ID: ";
	cin >> s.id;
	cin.ignore(); // Clear the newline character from the input buffer
	cout << "Enter student name: ";
	getline(cin, s.name);
	cout << "Enter student major: ";
	getline(cin, s.major);
	cout << "Enter student year: ";
	cin >> s.year;
	students.push_back(s);
}

void displayAllStudents(const vector<Student>& students) {
	cout << "ID\tName\tMajor\tYear\n";
	cout << "-----------------------------------\n";
	for (const auto& s : students) {
		cout << s.id << "\t" << s.name << "\t" << s.major << "\t" << s.year << "\n";
	}
}

void searchStudentByID(const vector<Student>& students) {
	string id;
	cout << "Enter student ID to search: ";
	cin >> id;
	bool found = false;
	for (const auto& s : students) {
		if (s.id == id) {
			cout << "ID: " << s.id << ", Name: " << s.name << ", Major: " << s.major << ", Year: " << s.year << "\n";
			found = true;
			break;
		}
	}
	if (!found) {
		cout << "Student with ID " << id << " not found.\n";
	}
}

void deleteStudent(vector<Student>& students) {
    string id;
    cout << " Enter the student number to delete : ";
    cin >> id;
    bool found = false;
    for (auto it = students.begin(); it != students.end(); ++it) {
        if (it->id == id) {
            students.erase(it);
            saveStudentsToFile(students);
            cout << "The student has been deleted.\n";
            found = true;
            break;
        }
    }
    if (!found) {
        cout << "The student is not present.\n";
    }
}

void menu() {
    vector<Student> students;
    loadStudentsFromFile(students);

    int choice;
    do {
        cout << "\nUniversity student management system\n";
        cout << "1. Add a new student\n";
        cout << "2. View all students\n";
        cout << "3. Search for a student by ID number\n";
        cout << "4. Delete a student\n";
        cout << "5. Exit\n";
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
        case 1: addStudent(students); break;
        case 2: displayAllStudents(students); break;
        case 3: searchStudentByID(students); break;
        case 4: deleteStudent(students); break;
        case 5: cout << " The program has terminated.\n"; break;
        default: cout << " Invalid option.\n"; break;
        }
    } while (choice != 5);
}
	
int main()
{
    menu();
}
