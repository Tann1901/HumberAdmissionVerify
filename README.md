## Humber Admission Verification Project

### Requirements
Writing Java code for an admission process for a college (Humber College) along with the generation of reports based on the admitted students' data with specific requirements as of:
1. User Authentication Requirements: user will login into the app, password requires to be >= 10 characters, at least one upper case letter, include 2-3 numbers and one special character. If the password correct then continue, else, give 3 more attempts.
2. Student Admission Requirements: enter the number of students to for verifying. Quatity is between 1-50, input also names and grades accordingly
3. Grading and GPA Calculation Requirements: collects grades for multiple subjects (maths, science, language, drama, music, biology) for each student and calculates their GPA. The requirement for grading and GPA calculation will allow the system to assign the student to Engineer (90-100), Business(80-90), Law(70-80) school respectively. If the score is under 70, they are not accepted.

After that, reports are generated including:

Report 1: Displays school assignments for each student.

Report 2: Shows the total number of students accepted in each school.

Report 3: Displays the number of students not accepted.

Report 4: Provides the average GPA for each school.

### UML Diagram
![image](https://github.com/Tann1901/HumberAdmissionVerify/assets/108020327/99ce4c42-eb60-4ad3-99a1-723275eb003c)

### HumberAdmissionVerify
```
/PRINT WELCOME 

public class HumberCollege {
	public static void main(String[] args) {
		
		displayWelcome();
		
		Admission admission = new Admission();
		admission.admitStdentsToSchools();
		
		Report report = new Report();
		report.getReport1(admission.getNamesOfStudents(), admission.getStudentsSchoolName());
		report.getReport2and3(admission.getStudentsSchoolName());
		report.getReport4(admission.getGpa(), admission.getStudentsSchoolName());	
	}
	// Print welcome message
	private static void displayWelcome() {
		System.out.println(" Welcome in Humber College \n");
	}
}
```
```
//CREATE CONDITIONS

import java.util.Scanner;
public class Admission extends AdmissionEntity {
	public void admitStdentsToSchools() {
		
		Scanner scan = new Scanner(System.in);
		boolean login = userlogin(scan);
		if (login == true) 
		{
			int noOfStudents = noOfStudents(scan);
			if (noOfStudents > 0) 
			{
				enterNamesOfStudent(noOfStudents, scan);
				
				double[][] marksOfStudents = getMarksOfStudents(scan, noOfStudents);
				scan.close();
				gpaCalculator(marksOfStudents);
				assignStudentsToSchool();
			}
		}
	}
```
```
//VALIDATE USER LOGIN (1)

// Validate user login
	private static boolean userlogin(Scanner scan) {
		boolean loginSuccessful = false;
		System.out.println("To login please enter your password.\n " + "(The password should satisfy the following criteria:)"
				+ " \n 1. Minimum 10 characters" + " \n 2. At least one upper case letter"
				+ " \n 3. Two or three numbers " + " \n 4. one special character  [!,@,#,$,%,^,&,*,(,)]");
		
		int i = 0;
		for (i = 0; i < 3; i++) { // Allow user 3 attempts
			if (i >= 0) {
				System.out.println("\n Please enter your Password!  ");
				String pass = scan.nextLine();
				// check if pass length is < 10
				if (pass.length() < 10) {
					System.out.println("Invalid password! Your length is less than 10 characters!");
				}
// Check if pass length > 10
if (pass.length() >= 10) {
	int uppercount = 0;
	int spCharCount = 0;
	int numcount = 0;
	for (int j = 0; j < pass.length(); j++) {
		char passchar = pass.charAt(j);
		if (passchar >= 'A' && passchar <= 'Z') {
		uppercount += 1;
		}
		if (Character.isDigit(passchar)) {
		numcount += 1;
		}
		if (passchar == '!' || passchar == '@' || passchar == '#' || passchar == '$' || passchar == '%'
		|| passchar == '^' || passchar == '&' || passchar == '*' || passchar == '('
		|| passchar == ')') {
		spCharCount += 1;
		}
	}
		if (uppercount == 0) {
		System.out.println("Should contain atleast One UPPERCASE Letter");
		}
if (numcount < 2 || numcount > 3) {
		System.out.println("Should contain 2-3 Numbers Only");
		}
		if (spCharCount != 1) {
		System.out.println("Should contain atleast One Special Character");
		}
		if ((uppercount > 0) && (numcount > 1 && numcount < 4) && (spCharCount == 1)) {
		System.out.println("Login Successful! ");
		loginSuccessful = true;
		break;
		}
		if (i == 2) {
		System.out.println("You have exhausted all your 3 tries! Please try again later.");
		System.exit(1);
		}
		} else if (i == 2) {
		System.out.println("You have exhausted all your 3 tries! Please try again later.");
		System.exit(1);
		}
	}
}
	return loginSuccessful;
	}
```
```
//INPUT FROM 1-50

private static int noOfStudents(Scanner scan) {
		int noOfstudents = 0;
		boolean isNoValid = false;
		for (int i = 0; i < 3; i++) {
			System.out.println("Please enter the number of students between 1 to 50 ");
			noOfstudents = scan.nextInt();
			if (noOfstudents > 0 && noOfstudents <= 50) {
				isNoValid = true;
				break;
			} else {
				System.out.println("Please enter a correct number (between 1-50)");
				noOfstudents = 0;
			}
		}
		if (!isNoValid) {
			System.out.println("You have exhausted all your 3 tries! Please try again later.");
			System.exit(1);
		}
		return noOfstudents;
	}
```
```
//ENTER NAME REGARDS OF STUDENT NUMBER 

private void enterNamesOfStudent(int noOfStudents, Scanner scan) {
		String[] studentNames = new String[noOfStudents];
		for (int i = 0; i < noOfStudents; i++) {
			System.out.println("Enter the name of student");
			studentNames[i] = scan.next();
		}
		setNamesOfStudents(studentNames);
	}
private void enterNamesOfStudent(int noOfStudents, Scanner scan) {
		String[] studentNames = new String[noOfStudents];
		for (int i = 0; i < noOfStudents; i++) {
			System.out.println("Enter the name of student");
			studentNames[i] = scan.next();
		}
		setNamesOfStudents(studentNames);
	}
	private double[][] getMarksOfStudents(Scanner scan, int noOfStudents) {
		
		String[] namesOfStudents = getNamesOfStudents();
		
		double[][] marksOfStudents = new double[noOfStudents][6]; //m*n where m=number of student and n= marks of that student (6 subjects)
		double mathsMarks;
		double scienceMarks;
		double langMarks;
		double dramaMarks;
		double musicMarks;
		double biologyMarks;
for (int i = 0; i < noOfStudents; i++) {
			System.out.println("Please enter the marks for " + namesOfStudents[i] + " in between 0 	- 100 ");
			System.out.print("Enter the Maths Marks: ");
			mathsMarks = scan.nextDouble();
			if(mathsMarks > 0 && mathsMarks<=100)
				marksOfStudents[i][0] = mathsMarks;
			else
				marksOfStudents[i][0] = 0;
			
			System.out.print("Enter the Science Marks: ");
			scienceMarks = scan.nextDouble();
			if(scienceMarks > 0 && scienceMarks<=100)
				marksOfStudents[i][1] = scienceMarks;
			else
				marksOfStudents[i][1] = 0;

			System.out.print("Enter the Language Marks: ");
			langMarks = scan.nextDouble();
			if(langMarks > 0 && langMarks<=100)
				marksOfStudents[i][2] = langMarks;
			else
				marksOfStudents[i][2] = 0;
			
			System.out.print("Enter the Drama Marks: ");
			dramaMarks = scan.nextDouble();
			if(dramaMarks > 0 && dramaMarks<=100)
				marksOfStudents[i][3] = dramaMarks;
			else
				marksOfStudents[i][3] = 0;
```
```
//GPA CALCULATION
private void gpaCalculator(double[][] marksOfStudents) {
		
double[] gpaCal = new double[marksOfStudents.length];

// declaring the credit of all the subjects
int maths_credit = 4, science_credit = 5, language_credit = 4, drama_credit = 3, music_credit = 2, biology_credit = 4;
		
int totalCreditMarks = maths_credit + science_credit + language_credit + drama_credit + music_credit + biology_credit;
		
// this will calculate the GPA according the formula provided
for (int i = 0; i < marksOfStudents.length; i++) {
	gpaCal[i] = ((marksOfStudents[i][0] * maths_credit) + (marksOfStudents[i][1] * science_credit) + (marksOfStudents[i][2] 		* language_credit)+ (marksOfStudents[i][3] * drama_credit) + (marksOfStudents[i][4] * music_credit) + 		(marksOfStudents[i][5] * biology_credit))/ (totalCreditMarks);
		}
	setGpa(gpaCal);
	}
	private void assignStudentsToSchool() {
		double[] gpa = getGpa();
		String[] schoolNamesOfStudents = new String[gpa.length];
```
```
//CHECK WHICH SCHOOL TO BE ASSIGNED

		for (int i = 0; i < gpa.length; i++) {
			if (gpa[i] >= 90 && gpa[i] <= 100) {
				schoolNamesOfStudents[i] = "School of Engineering";
			} else if (gpa[i] >= 80 && gpa[i] < 90) {
				schoolNamesOfStudents[i] = "School of Business";
			} else if (gpa[i] >= 70 && gpa[i] < 80) {
				schoolNamesOfStudents[i] = "Law School";
			} else {
				schoolNamesOfStudents[i] = "Not accepted";
			}
		}
		setStudentsSchoolName(schoolNamesOfStudents);
	}
}
```
```
//CHECK WHICH SCHOOL TO BE ASSIGNED

public class AdmissionEntity {
	private String[] namesOfStudents;
	private String[] studentsSchoolName;
	private double[] gpa;
	
	public String[] getNamesOfStudents() {
		return namesOfStudents;
	}
	public void setNamesOfStudents(String[] namesOfStudents) {
		this.namesOfStudents = namesOfStudents;
	}
	public String[] getStudentsSchoolName() {
		return studentsSchoolName;
	}
public void setStudentsSchoolName(String[] studentsSchoolName) {
		this.studentsSchoolName = studentsSchoolName;
	}
	public double[] getGpa() {
		return gpa;
	}
	public void setGpa(double[] gpa) {
		this.gpa = gpa;
	}
}
```
### Report Generation
```
//REPORT 1
/
public class Report {
	
	public void getReport1(String[] namesOfStudents, String[] studentsSchoolName) {
		
		System.out.println("\n Report 1 Starts \n");
		for (int i = 0; i < namesOfStudents.length; i++) {
			System.out.println("Student " +namesOfStudents[i] + " has been accepted in " + studentsSchoolName[i]);
		}
		System.out.println(" Report 1 Ends \n");
	}
```
```
//REPORT 2 & 3
public void getReport2and3(String[] studentsSchoolName) {
		int noOfStudentsInEngg = 0;
		int noOfStudentsInBusiness = 0;
		int noOfStudentsInLaw = 0;
		int noOfStudentsNA = 0;
		
		for (int i = 0; i < studentsSchoolName.length; i++) {
			if(studentsSchoolName[i].equals("School of Engineering")) {
				noOfStudentsInEngg++;
			} else if(studentsSchoolName[i].equals("School of Business")) {
				noOfStudentsInBusiness++;
			} else if(studentsSchoolName[i].equals("Law School")) {
				noOfStudentsInLaw++;
			} else {
				noOfStudentsNA++;
			}
		}
		
		System.out.println("\n Report 2 Starts  \n");
		System.out.println("Total number of students accepted in Humber College are " + 							         (noOfStudentsInEngg+noOfStudentsInBusiness+noOfStudentsInLaw));
		System.out.println("Number of students accepted in School of Engineering are " + noOfStudentsInEngg);
		System.out.println("Number of students accepted in School of Business are " + noOfStudentsInBusiness);
		System.out.println("Number of students accepted in Law School are " + noOfStudentsInLaw);
		System.out.println(" Report 2 Ends  \n");
		
		System.out.println("\n Report 3 Starts  \n");
		System.out.println("Total number of students not accepted in Humber College are " + (noOfStudentsNA));
		System.out.println(" Report 3 Ends  \n");
	}
```
```
//REPORT 4
public void getReport4(double[] gpa, String[] studentsSchoolName) {
		int noOfStudentsInEngg = 0;
		int noOfStudentsInBusiness = 0;
		int noOfStudentsInLaw = 0;
		double gpaEngg = 0;
		double gpaBusiness = 0;
		double gpaLaw = 0;
		
		for (int i = 0; i < studentsSchoolName.length; i++) {
			if(studentsSchoolName[i].equals("School of Engineering")) {
				noOfStudentsInEngg++;
				gpaEngg += gpa[i];
			} else if(studentsSchoolName[i].equals("School of Business")) {
				noOfStudentsInBusiness++;
				gpaBusiness += gpa[i];
			} if(studentsSchoolName[i].equals("Law School")) {
				noOfStudentsInLaw++;
				gpaLaw += gpa[i];
			}
		}
		
		System.out.println("\n Report 4 Starts  \n");
		System.out.println("Average GPA of School of Engineering is " + (gpaEngg/noOfStudentsInEngg));
		System.out.println("Average GPA of School of Business is " + (gpaBusiness/noOfStudentsInBusiness));
		System.out.println("Average GPA of Law School is " + (gpaLaw/noOfStudentsInLaw));
		System.out.println(" Report 4 Ends  \n");
	}
}
```
### OUTPUT

1.Login with Valid Password
![image](https://github.com/Tann1901/HumberAdmissionVerify/assets/108020327/67fdd204-2207-4cf8-993a-c91d17db9c0d)
2.Entering Student Names & Subject Marks
![image](https://github.com/Tann1901/HumberAdmissionVerify/assets/108020327/d5c0681d-4da6-45f8-b449-1935ef913386)
![image](https://github.com/Tann1901/HumberAdmissionVerify/assets/108020327/61d19d9f-71b9-42f6-8f24-429f2083e56e)
3.Wrong Password Rejection:
![image](https://github.com/Tann1901/HumberAdmissionVerify/assets/108020327/626f1d6d-909e-485c-a7d1-77c048c26221)
