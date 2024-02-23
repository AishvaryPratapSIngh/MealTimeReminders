from django.core.management.base import BaseCommand
from django.utils import timezone
import datetime
import threading

class Command(BaseCommand):
    help = 'Schedule meal reminders'

    def handle(self, *args, **kwargs):
        # Get user input for meal times
        print("Welcome to Meal Time Reminder!")
        breakfast_time = self.get_user_input("Please enter the time for breakfast (HH:MM format):")
        lunch_time = self.get_user_input("Please enter the time for lunch (HH:MM format):")
        dinner_time = self.get_user_input("Please enter the time for dinner (HH:MM format):")

        # Schedule reminders for each meal time
        self.schedule_reminder("Breakfast", breakfast_time)
        self.schedule_reminder("Lunch", lunch_time)
        self.schedule_reminder("Dinner", dinner_time)

        print("Meal Time Reminders scheduled successfully!")

    def get_user_input(self, message):
        while True:
            try:
                time_input = input(message)
                hours, minutes = map(int, time_input.split(":"))
                if 0 <= hours < 24 and 0 <= minutes < 60:
                    return datetime.time(hours, minutes)
                else:
                    print("Invalid time format or values. Please use HH:MM format and ensure valid values.")
            except ValueError:
                print("Invalid time format or values. Please use HH:MM format and ensure valid values.")

    def calculate_delay(self, meal_time):
        current_time = timezone.now().time()
        target_time = datetime.datetime.combine(timezone.now().date(), meal_time)
        if target_time < current_time:
            target_time += datetime.timedelta(days=1)
        return (target_time - datetime.datetime.now()).total_seconds()

    def schedule_reminder(self, meal, meal_time):
        delay = self.calculate_delay(meal_time)
        threading.Timer(delay, self.remind, args=[meal]).start()

    def remind(self, meal):
        print(f"It's time for {meal}! Don't forget to order your meal.")
