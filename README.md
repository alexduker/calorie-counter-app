# calorie-counter-app
import json

class CalorieCounter:
    def __init__(self, file_name="calories.json"):
        self.file_name = file_name
        self.load_data()

    def load_data(self):
        try:
            with open(self.file_name, "r") as file:
                self.data = json.load(file)
        except (FileNotFoundError, json.JSONDecodeError):
            self.data = {"meals": [], "total_calories": 0}

    def save_data(self):
        with open(self.file_name, "w") as file:
            json.dump(self.data, file, indent=4)

    def add_meal(self, meal_name, calories):
        self.data["meals"].append({"meal": meal_name, "calories": calories})
        self.data["total_calories"] += calories
        self.save_data()
        print(f"Added {meal_name} ({calories} kcal)")

    def list_meals(self):
        if not self.data["meals"]:
            print("No meals recorded.")
            return
        for idx, meal in enumerate(self.data["meals"], start=1):
            print(f"{idx}. {meal['meal']} - {meal['calories']} kcal")
        print(f"Total Calories: {self.data['total_calories']} kcal")

    def reset_data(self):
        self.data = {"meals": [], "total_calories": 0}
        self.save_data()
        print("Calorie data has been reset.")

# Example usage
def main():
    counter = CalorieCounter()
    while True:
        print("\nCalorie Counter App")
        print("1. Add Meal")
        print("2. List Meals")
        print("3. Reset Data")
        print("4. Exit")
        choice = input("Select an option: ")
        
        if choice == "1":
            meal_name = input("Enter meal name: ")
            calories = int(input("Enter calories: "))
            counter.add_meal(meal_name, calories)
        elif choice == "2":
            counter.list_meals()
        elif choice == "3":
            counter.reset_data()
        elif choice == "4":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
