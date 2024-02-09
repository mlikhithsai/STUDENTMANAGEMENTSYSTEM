import javafx.application.Application;
import javafx.beans.property.SimpleIntegerProperty;
import javafx.beans.property.SimpleStringProperty;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.control.cell.PropertyValueFactory;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;
import javafx.collections.FXCollections;


import java.io.*;
import java.util.ArrayList;
import java.util.Arrays;

class Student implements Serializable {
    private SimpleStringProperty name;
    private SimpleIntegerProperty rollNumber;
    private SimpleStringProperty grade;

    public Student(String name, int rollNumber, String grade) {
        this.name = new SimpleStringProperty(name);
        this.rollNumber = new SimpleIntegerProperty(rollNumber);
        this.grade = new SimpleStringProperty(grade);
    }

    public String getName() {
        return name.get();
    }

    public SimpleStringProperty nameProperty() {
        return name;
    }

    public int getRollNumber() {
        return rollNumber.get();
    }

    public SimpleIntegerProperty rollNumberProperty() {
        return rollNumber;
    }

    public String getGrade() {
        return grade.get();
    }

    public SimpleStringProperty gradeProperty() {
        return grade;
    }

    @Override
    public String toString() {
        return "Name: " + name.get() + ", Roll Number: " + rollNumber.get() + ", Grade: " + grade.get();
    }
}

public class StudentManagementSystem extends Application {
    private ArrayList<Student> students;
    private TableView<Student> tableView;
    private TextField nameField;
    private TextField rollNumberField;
    private TextField gradeField;

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        students = readStudentsFromFile(); // Read students from file

        // Set up the UI components
        GridPane gridPane = new GridPane();
        gridPane.setPadding(new Insets(10, 10, 10, 10));
        gridPane.setVgap(5);
        gridPane.setHgap(5);

        Label nameLabel = new Label("Name:");
        Label rollNumberLabel = new Label("Roll Number:");
        Label gradeLabel = new Label("Grade:");

        nameField = new TextField();
        rollNumberField = new TextField();
        gradeField = new TextField();

        Button addButton = new Button("Add Student");
        Button removeButton = new Button("Remove Student");
        Button searchButton = new Button("Search Student");
        Button displayButton = new Button("Display All Students");

        // Create a TableView to display students
        tableView = new TableView<>();
        TableColumn<Student, String> nameColumn = new TableColumn<>("Name");
		nameColumn.setCellValueFactory(cellData -> cellData.getValue().nameProperty());

		TableColumn<Student, Integer> rollNumberColumn = new TableColumn<>("Roll Number");
		rollNumberColumn.setCellValueFactory(cellData -> cellData.getValue().rollNumberProperty().asObject());

		TableColumn<Student, String> gradeColumn = new TableColumn<>("Grade");
		gradeColumn.setCellValueFactory(cellData -> cellData.getValue().gradeProperty());


        tableView.getColumns().addAll(Arrays.asList(nameColumn, rollNumberColumn, gradeColumn));

        addButton.setOnAction(e -> addStudent(nameField.getText(), rollNumberField.getText(), gradeField.getText()));
        removeButton.setOnAction(e -> removeStudent());
        searchButton.setOnAction(e -> searchStudent(rollNumberField.getText()));
        displayButton.setOnAction(e -> displayAllStudents());

        // Add components to the gridPane
        gridPane.add(nameLabel, 0, 0);
        gridPane.add(nameField, 1, 0);
        gridPane.add(rollNumberLabel, 0, 1);
        gridPane.add(rollNumberField, 1, 1);
        gridPane.add(gradeLabel, 0, 2);
        gridPane.add(gradeField, 1, 2);
        gridPane.add(addButton, 0, 3);
        gridPane.add(removeButton, 1, 3);
        gridPane.add(searchButton, 0, 4);
        gridPane.add(displayButton, 1, 4);
        gridPane.add(tableView, 0, 5, 2, 1);

        Scene scene = new Scene(gridPane, 400, 400);
        primaryStage.setTitle("Student Management System");
        primaryStage.setScene(scene);
        primaryStage.show();

        primaryStage.setOnCloseRequest(e -> writeStudentsToFile()); // Write students to file on application exit
    }

    private void addStudent(String name, String rollNumber, String grade) {
        // Validation and adding student to the list
        int rollNum = 0;
        try {
            rollNum = Integer.parseInt(rollNumber);
        } catch (NumberFormatException e) {
            showAlert("Invalid input", "Please enter a valid roll number.");
            return;
        }

        if (name.isEmpty() || grade.isEmpty()) {
            showAlert("Missing Information", "Please fill in all fields.");
            return;
        }

        Student student = new Student(name, rollNum, grade);
        students.add(student);

        // Update the TableView
        tableView.getItems().add(student);

        clearInputFields();
        showAlert("Student added", "Student added successfully.");
    }

    private void removeStudent() {
        // Remove selected student from the list
        Student selectedStudent = tableView.getSelectionModel().getSelectedItem();
        if (selectedStudent == null) {
            showAlert("No Selection", "Please select a student to remove.");
            return;
        }

        students.remove(selectedStudent);
        tableView.getItems().remove(selectedStudent);

        showAlert("Student removed", "Student removed successfully.");
    }

    private void searchStudent(String rollNumber) {
        // Search for student with given roll number
        int rollNum = 0;
        try {
            rollNum = Integer.parseInt(rollNumber);
        } catch (NumberFormatException e) {
            showAlert("Invalid input", "Please enter a valid roll number.");
            return;
        }

        for (Student student : students) {
            if (student.getRollNumber() == rollNum) {
                showAlert("Student found", student.toString());
                return;
            }
        }

        showAlert("Student not found", "No student found with the given roll number.");
    }

    private void displayAllStudents() {
    // Display all students in the TableView
    if (students.isEmpty()) {
        showAlert("No Students", "No students available.");
    } else {
        tableView.setItems(FXCollections.observableArrayList(students));
    }
}


    @SuppressWarnings("unchecked")
    private ArrayList<Student> readStudentsFromFile() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("students.dat"))) {
            Object readObject = ois.readObject();
            if (readObject instanceof ArrayList) {
                return (ArrayList<Student>) readObject;
            } else {
                throw new IOException("Invalid or empty file content.");
            }
        } catch (IOException | ClassNotFoundException e) {
            return new ArrayList<>();
        }
    }

    private void writeStudentsToFile() {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("students.dat"))) {
            oos.writeObject(students);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private void clearInputFields() {
        // Clear input fields after adding a student
        nameField.clear();
        rollNumberField.clear();
        gradeField.clear();
    }

    private void showAlert(String title, String content) {
        // Display an alert dialog
        Alert alert = new Alert(Alert.AlertType.INFORMATION);
        alert.setTitle(title);
        alert.setHeaderText(null);
        alert.setContentText(content);
        alert.showAndWait();
    }
}
