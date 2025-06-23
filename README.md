#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Patient {
    int id;
    char name[50];
    int age;
    char gender[10];
    char diagnosis[100];
};

void addPatient() {
    struct Patient p;
    FILE *fp = fopen("patients.dat", "ab");
    if (fp == NULL) {
        printf("Error opening file!\n");
        return;
    }

    printf("Enter Patient ID: ");
    scanf("%d", &p.id);
    printf("Enter Name: ");
    scanf(" %[^\n]", p.name);
    printf("Enter Age: ");
    scanf("%d", &p.age);
    printf("Enter Gender: ");
    scanf("%s", p.gender);
    printf("Enter Diagnosis: ");
    scanf(" %[^\n]", p.diagnosis);

    fwrite(&p, sizeof(struct Patient), 1, fp);
    fclose(fp);
    printf("Patient record added successfully.\n");
}

void displayPatients() {
    struct Patient p;
    FILE *fp = fopen("patients.dat", "rb");
    if (fp == NULL) {
        printf("No records found.\n");
        return;
    }

    printf("\n--- All Patients ---\n");
    while (fread(&p, sizeof(struct Patient), 1, fp)) {
        printf("ID: %d\nName: %s\nAge: %d\nGender: %s\nDiagnosis: %s\n\n",
               p.id, p.name, p.age, p.gender, p.diagnosis);
    }
    fclose(fp);
}

void searchPatient() {
    struct Patient p;
    int id, found = 0;
    FILE *fp = fopen("patients.dat", "rb");
    if (fp == NULL) {
        printf("No records found.\n");
        return;
    }

    printf("Enter Patient ID to search: ");
    scanf("%d", &id);

    while (fread(&p, sizeof(struct Patient), 1, fp)) {
        if (p.id == id) {
            printf("\nRecord Found:\n");
            printf("ID: %d\nName: %s\nAge: %d\nGender: %s\nDiagnosis: %s\n",
                   p.id, p.name, p.age, p.gender, p.diagnosis);
            found = 1;
            break;
        }
    }

    if (!found)
        printf("Patient with ID %d not found.\n", id);
    fclose(fp);
}

void menu() {
    int choice;
    do {
        printf("\nHospital Management System\n");
        printf("1. Add Patient\n");
        printf("2. Display Patients\n");
        printf("3. Search Patient\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: addPatient(); break;
            case 2: displayPatients(); break;
            case 3: searchPatient(); break;
            case 4: printf("Exiting...\n"); break;
            default: printf("Invalid choice!\n");
        }
    } while (choice != 4);
}

int main() {
    menu();
    return 0;
}
