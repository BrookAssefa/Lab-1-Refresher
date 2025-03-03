#include <iostream>
#include <fstream>
#include <iomanip>
#include <algorithm> // for sort function

using namespace std;

// Global constants for file names
const string INPUT_FILE = "input.txt";
const string OUTPUT_FILE = "output.txt";

// Constants for array sizes
const int MAX_SIZE = 1000;

// Function prototypes
void getData(int divBy5[], int divBy9[], int rest[], int &count5, int &count9, int &countRest);
void printArray(const int arr[], int size, const string &label);
void printStats(const int arr[], int size, const string &label);
int calculateSum(const int arr[], int size);
double calculateAverage(const int arr[], int size);
double calculateMedian(int arr[], int size);
void bubbleSort(int arr[], int size);
void writeToFile(const int divBy5[], int count5, const int divBy9[], int count9, const int rest[], int countRest);

int main() {
// Arrays to store numbers
int divBy5[MAX_SIZE], divBy9[MAX_SIZE], rest[MAX_SIZE];
int count5 = 0, count9 = 0, countRest = 0;

// Read data from the input file
getData(divBy5, divBy9, rest, count5, count9, countRest);

// Menu-driven program
int choice;
do {
cout << "\nMenu:\n";
cout << "1. Print values and stats for each array\n";
cout << "2. Print sorted values in each array\n";
cout << "3. Quit and write results to output file\n";
cout << "Enter your choice: ";
cin >> choice;

switch (choice) {
case 1:
printStats(divBy5, count5, "Numbers divisible by 5");
printStats(divBy9, count9, "Numbers divisible by 9");
printStats(rest, countRest, "Other numbers");
break;
case 2:
bubbleSort(divBy5, count5);
bubbleSort(divBy9, count9);
bubbleSort(rest, countRest);
printArray(divBy5, count5, "Sorted numbers divisible by 5");
printArray(divBy9, count9, "Sorted numbers divisible by 9");
printArray(rest, countRest, "Sorted other numbers");
break;
case 3:
writeToFile(divBy5, count5, divBy9, count9, rest, countRest);
cout << "Results written to " << OUTPUT_FILE << ". Exiting program.\n";
break;
default:
cout << "Invalid choice. Please try again.\n";
break;
}
} while (choice != 3);

return 0;
}

// Function to read data from the input file
void getData(int divBy5[], int divBy9[], int rest[], int &count5, int &count9, int &countRest) {
ifstream inputFile(INPUT_FILE);
if (!inputFile) {
cout << "Error: Unable to open input file.\n";
return;
}

int number;
while (inputFile >> number) {
if (number % 5 == 0) {
if (count5 < MAX_SIZE) divBy5[count5++] = number;
else cout << "Warning: Array for numbers divisible by 5 is full.\n";
} else if (number % 9 == 0) {
if (count9 < MAX_SIZE) divBy9[count9++] = number;
else cout << "Warning: Array for numbers divisible by 9 is full.\n";
} else {
if (countRest < MAX_SIZE) rest[countRest++] = number;
else cout << "Warning: Array for other numbers is full.\n";
}
}

inputFile.close();
}

// Function to print an array
void printArray(const int arr[], int size, const string &label) {
if (size == 0) {
cout << label << " array is empty.\n";
return;
}

cout << label << ":\n";
for (int i = 0; i < size; i++) {
cout << arr[i] << " ";
}
cout << "\n";
}

// Function to print statistics for an array
void printStats(const int arr[], int size, const string &label) {
if (size == 0) {
cout << label << " array is empty.\n";
return;
}

cout << label << ":\n";
printArray(arr, size, "Values");
cout << "Sum: " << calculateSum(arr, size) << "\n";
cout << "Average: " << fixed << setprecision(2) << calculateAverage(arr, size) << "\n";
cout << "Median: " << calculateMedian(arr, size) << "\n";
}

// Function to calculate the sum of an array
int calculateSum(const int arr[], int size) {
int sum = 0;
for (int i = 0; i < size; i++) {
sum += arr[i];
}
return sum;
}

// Function to calculate the average of an array
double calculateAverage(const int arr[], int size) {
if (size == 0) return 0.0;
return static_cast<double>(calculateSum(arr, size)) / size;
}

// Function to calculate the median of an array
double calculateMedian(int arr[], int size) {
if (size == 0) return 0.0;

// Sort the array
bubbleSort(arr, size);

if (size % 2 == 0) {
return (arr[size / 2 - 1] + arr[size / 2]) / 2.0;
} else {
return arr[size / 2];
}
}

// Function to sort an array using bubble sort
void bubbleSort(int arr[], int size) {
for (int i = 0; i < size - 1; i++) {
for (int j = 0; j < size - i - 1; j++) {
if (arr[j] > arr[j + 1]) {
// Swap without using a separate function
int temp = arr[j];
arr[j] = arr[j + 1];
arr[j + 1] = temp;
}
}
}
}

// Function to write results to the output file
void writeToFile(const int divBy5[], int count5, const int divBy9[], int count9, const int rest[], int countRest) {
ofstream outputFile(OUTPUT_FILE);
if (!outputFile) {
cout << "Error: Unable to open output file.\n";
return;
}
