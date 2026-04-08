# SMART HEALTH TRACKING SYSTEM

import json
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# -------------------- CLASS --------------------
class Patient:
    def __init__(self, name, age, gender, blood, height, weight, phone):
        self.name = name
        self.age = age
        self.gender = gender
        self.blood = blood
        self.height = height
        self.weight = weight
        self.phone = phone
        self.vitals = []

    def display(self):
        print(f"\nPatient Details:")
        print(f"Name: {self.name}")
        print(f"Age: {self.age}")
        print(f"Gender: {self.gender}")
        print(f"Blood Group: {self.blood}")
        print(f"Height: {self.height}")
        print(f"Weight: {self.weight}")
        print(f"Phone: {self.phone}")

    def add_vitals(self):
        try:
            bp = int(input("Enter BP: "))
            hr = int(input("Enter Heart Rate: "))
            self.vitals.append((bp, hr))
            print("Vitals Added!")
        except:
            print("Invalid input!")

    def show_vitals(self):
        print("\nVitals Records:")
        for v in self.vitals:
            print("BP:", v[0], "HR:", v[1])

    def analyze(self):
        if len(self.vitals) == 0:
            print("No data to analyze")
            return
        
        bp_values = [v[0] for v in self.vitals]
        print("Average BP:", np.mean(bp_values))

        # Pandas DataFrame
        df = pd.DataFrame(self.vitals, columns=["BP", "HR"])
        print("\nDataFrame:\n", df)

        # Graph
        plt.plot(bp_values)
        plt.title("BP Trend")
        plt.xlabel("Records")
        plt.ylabel("BP")
        plt.show()

    def save_to_file(self):
        data = {
            "name": self.name,
            "age": self.age,
            "gender": self.gender,
            "blood": self.blood,
            "height": self.height,
            "weight": self.weight,
            "phone": self.phone,
            "vitals": self.vitals
        }

        with open("patient.json", "w") as f:
            json.dump(data, f)

        print("Data saved to file!")


# -------------------- MENU SYSTEM --------------------
def main():
    patient = None

    while True:
        print("\n===== SMART HEALTH SYSTEM =====")
        print("1. Add Patient")
        print("2. View Patient")
        print("3. Add Vitals")
        print("4. Show Vitals")
        print("5. Analyze Data")
        print("6. Save to File")
        print("7. Exit")

        try:
            choice = int(input("Enter choice: "))
        except:
            print("Invalid input")
            continue

        if choice == 1:
            name = input("Enter Name: ")
            age = int(input("Enter Age: "))
            gender = input("Enter Gender: ")
            blood = input("Enter Blood Group: ")
            height = float(input("Enter Height: "))
            weight = float(input("Enter Weight: "))
            phone = input("Enter Phone Number: ")

            patient = Patient(name, age, gender, blood, height, weight, phone)
            print("Patient Added Successfully!")

        elif choice == 2:
            if patient:
                patient.display()
            else:
                print("No patient found")

        elif choice == 3:
            if patient:
                patient.add_vitals()
            else:
                print("Add patient first")

        elif choice == 4:
            if patient:
                patient.show_vitals()
            else:
                print("No data")

        elif choice == 5:
            if patient:
                patient.analyze()
            else:
                print("No data")

        elif choice == 6:
            if patient:
                patient.save_to_file()
            else:
                print("No data")

        elif choice == 7:
            print("Exiting...")
            break

        else:
            print("Invalid choice")


# -------------------- RUN PROGRAM --------------------
main()
