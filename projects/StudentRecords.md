---
layout: project
type: project
image: img/Student.jpg
title: "Student Record Keeper"
date: 2024/03/25
published: true
summary: "I developed code in my ICS 212 class that lets you input student information and also keep a record of it."
---

![Student Records](https://tx02201707.schoolwires.net/cms/lib/TX02201707/Centricity/Domain/5513/records.jpg)

For one of our ICS 212 assignments, we created code that lets you input an imaginary student's name, age, and GPA and keeps a record of it. This code allows you to see the entire record database to see which students were registered. It's pretty neat and could have many different versions of similar code. Here is a snippet of my code.

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
```
