# assignment-1
# ===========================================
# Daily Calorie Tracker (User Input Version)
# ===========================================

import datetime

def get_meal_info():
    meal_names = []
    meal_calories = []

    # Input validation for meal count
    while True:
        try:
            num_meals = int(input("How many meals did you have today? "))
            if num_meals <= 0:
                print("❌ Please enter a positive number.")
                continue
            break
        except ValueError:
            print("⚠️ Invalid input. Please enter a number.")

    print("\nNow, enter your meals and their calorie values:\n")
    for i in range(num_meals):
        meal_name = input(f"Enter name for meal {i + 1}: ").strip()
        while True:
            try:
                calories = float(input(f"Enter calories for '{meal_name}': "))
                if calories <= 0:
                    print("❌ Calories must be a positive number.")
                    continue
                break
            except ValueError:
                print("⚠️ Please enter a valid number for calories.")

        meal_names.append(meal_name)
        meal_calories.append(calories)

    return meal_names, meal_calories


def calculate_calories(meal_calories):
    total_calories = sum(meal_calories)
    average_calories = total_calories / len(meal_calories)
    return total_calories, average_calories


def check_limit(total_calories, daily_limit):
    if total_calories > daily_limit:
        return f"⚠️ Warning: You have exceeded your daily limit of {daily_limit} calories!"
    else:
        return f"✅ Great job! You are within your daily calorie limit of {daily_limit}."


def save_session_log(meal_names, meal_calories, total_calories, average_calories, daily_limit):
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d_%H-%M-%S")
    filename = f"calorie_log_{timestamp}.txt"

    # ✅ FIX: Use UTF-8 encoding for Windows compatibility
    with open(filename, "w", encoding="utf-8") as file:
        file.write("===========================================\n")
        file.write("        Daily Calorie Tracker Log\n")
        file.write("===========================================\n")
        file.write(f"Date & Time: {timestamp}\n\n")
        file.write(f"{'Meal Name':<20}{'Calories'}\n")
        file.write("-------------------------------------------\n")
        for meal, cal in zip(meal_names, meal_calories):
            file.write(f"{meal:<20}{cal}\n")
        file.write("-------------------------------------------\n")
        file.write(f"{'Total':<20}{total_calories}\n")
        file.write(f"{'Average':<20}{average_calories:.2f}\n")
        file.write("-------------------------------------------\n")
        file.write(check_limit(total_calories, daily_limit) + "\n")
        file.write("===========================================\n")

    print(f"\n✅ Report saved successfully as '{filename}'")


def main():
    print("===========================================")
    print("       Welcome to the Daily Calorie Tracker")
    print("===========================================")
    print("This tool helps you record your meals, calculate total calories,")
    print("and check whether you've exceeded your daily calorie limit.\n")

    # Get user-entered meal info
    meal_names, meal_calories = get_meal_info()
    print("\nMeals recorded successfully!\n")

    # Calculate stats
    total_calories, average_calories = calculate_calories(meal_calories)
    print(f"Total Calorie Intake: {total_calories}")
    print(f"Average Calories per Meal: {average_calories:.2f}\n")

    # Get daily calorie limit
    while True:
        try:
            daily_limit = float(input("Enter your daily calorie limit: "))
            if daily_limit <= 0:
                print("❌ Please enter a positive number.")
                continue
            break
        except ValueError:
            print("⚠️ Please enter a valid number for daily limit.")

    print("\n" + check_limit(total_calories, daily_limit))

    # Summary table
    print("\n-------------------------------------------")
    print("           Daily Calorie Summary")
    print("-------------------------------------------")
    print(f"{'Meal Name':<20}{'Calories'}")
    print("-------------------------------------------")
    for meal, cal in zip(meal_names, meal_calories):
        print(f"{meal:<20}{cal}")
    print("-------------------------------------------")
    print(f"{'Total':<20}{total_calories}")
    print(f"{'Average':<20}{average_calories:.2f}")
    print("-------------------------------------------\n")

    # Option to save session
    save_option = input("Do you want to save this session report? (yes/no): ").strip().lower()
    if save_option == "yes":
        save_session_log(meal_names, meal_calories, total_calories, average_calories, daily_limit)
    else:
        print("\n📁 Report not saved. Session ended.")


if __name__ == "__main__":
    main()
2
