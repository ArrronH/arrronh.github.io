---
layout: project
type: project
image: img/Student.jpg
title: "Student Record Keeper"
date: 2024/03/25
published: true
labels:
  - C
  - Java
summary: "I developed code in my ICS212 class that lets you input student information and keep a record of it."
---

For one of our ICS212 assignments, we created code that lets you input an imaginary student's name, age, and GPA and keeps a record of it. This code allows you to see the entire record database to see which students were registered. It's pretty neat and could have many different versions of similar code. Here is a snippet of my code.

```cpp
int main(){
    int i = 0;
    Student studentX = {-1, "", "", 0, 0.0}; //create a Student struct with default values
    Student studentArray[MAX_RECORDS] = {-1, "", "", 0, 0.0}; //array of Student structs
    FILE *filePointer = NULL;
    char *fileName = "students.dat"; 
   
    //variables for user input and editing
    int readError = 0;
    int recordNumber = 0;
    char fieldChoice;
    char continueEditing = 'y';
   
    //open the file for reading/writing
    filePointer = fopen(fileName, "rb+"); //"rb+" to read and write from a binary file
    //check if file opening was successful 
    if(filePointer == NULL){
        printf("File \"%s\" could not be opened.\n", fileName); 
        exit(1); //end the program
    }
     
    //read student records from the file
    while(!feof(filePointer)){
        readError = fread(&studentX, sizeof(Student), 1, filePointer);
        if(1 == readError) {
            studentArray[i] = studentX;
            i++;
        }
    }
    
    //loop for editing records
    while ((continueEditing == 'y' || continueEditing == 'Y') && recordNumber != 20) {
        printHeader(); //print the header for student records

        //print existing student records
        for(i = 0; i< MAX_RECORDS; i++) {
            if(-1 != studentArray[i].number) {
                printStudent(studentArray[i]);
            }
        }

        //get the record number to edit
        recordNumber = getRecordNumber();

        //validate the record number
        if(recordNumber < 0) {
            puts("ERROR: No negative numbered records exist!");
            continue;
        } else if (recordNumber >= MAX_RECORDS) {
            printf("ERROR: No records %i or above exist!", MAX_RECORDS);
            continue;
        } else if (-1 == studentArray[recordNumber].number) {
            printf("ERROR: Student #%i does not exist.", recordNumber);
            continue;
        }

        printf("You selected record #%d:\n", recordNumber);
        printStudent(studentArray[recordNumber]);
        fieldChoice = getFieldChoice();

        //switch statement to handle field editing based on user choice
        switch(fieldChoice) {
            case 'f':
                printf("Enter first name: ");
                scanf("%s", studentArray[recordNumber].first);
                break;
            case 'l':
                printf("Enter last name: ");
                scanf("%s", studentArray[recordNumber].last);
                break;
            case 'a':
                printf("Enter age: ");
                scanf("%d", &studentArray[recordNumber].age); //getdouble function to get age
                break;
            case 'g':
                printf("Enter gpa: ");
                scanf("%lf", &studentArray[recordNumber].gpa); //getdouble function to get gpa
                break;
            default:
                printf("ERROR: '%c' is not a valid answer!\n", fieldChoice);
                continue;
        }

        //set file pointer back to the beginning of the file
        fseek(filePointer, 0, SEEK_SET);
        //write the modified student array back to the file
        fwrite(studentArray, sizeof(Student), MAX_RECORDS, filePointer);

        //ask the user if they want to continue editing the records
        printf("Keep editing records? Yes(y) or No(n)? ");
        scanf(" %c", &continueEditing);
    }

    fclose(filePointer); //close the file
    printf("The file \"%s\" was successfully updated.\n", fileName);

    return 0;
}
```
