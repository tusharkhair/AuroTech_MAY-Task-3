import time
from datetime import datetime, timedelta

class Timer:
    def __init__(self):
        self.start_time = None
        self.end_time = None
        self.elapsed_time = None
        self.running = False

    def start(self):
        if not self.running:
            self.start_time = datetime.now()
            self.running = True

    def stop(self):
        if self.running:
            self.end_time = datetime.now()
            self.elapsed_time = self.end_time - self.start_time
            self.running = False

    def reset(self):
        self.start_time = None
        self.end_time = None
        self.elapsed_time = None
        self.running = False

    def get_elapsed_time(self):
        if self.running:
            return datetime.now() - self.start_time
        else:
            return self.elapsed_time

class CountdownTimer(Timer):
    def __init__(self, duration):
        super().__init__()
        self.duration = duration

    def start(self):
        super().start()
        self.end_time = datetime.now() + timedelta(seconds=self.duration)

    def is_expired(self):
        if self.end_time < datetime.now():
            return True
        else:
            return False

class Stopwatch(Timer):
    pass

def print_timer(timer):
    if isinstance(timer, CountdownTimer):
        if timer.is_expired():
            print("Time's up!")
        else:
            elapsed_time = timer.get_elapsed_time()
            minutes, seconds = divmod(elapsed_time.seconds, 60)
            print(f"{minutes:02d}:{seconds:02d}")
    elif isinstance(timer, Stopwatch):
        elapsed_time = timer.get_elapsed_time()
        minutes, seconds = divmod(elapsed_time.seconds, 60)
        print(f"{minutes:02d}:{seconds:02d}.{elapsed_time.microseconds // 1000:03d}")

def main():
    print("Select a timer:")
    print("1. Countdown Timer")
    print("2. Stopwatch")
    choice = int(input("Enter the number of your choice: "))
    if choice == 1:
        duration = int(input("Enter the duration of the countdown timer in seconds: "))
        timer = CountdownTimer(duration)
    elif choice == 2:
        timer = Stopwatch()
    else:
        print("Invalid choice")
        return

    while True:
        print_timer(timer)
        time.sleep(0.1)
        if timer.running:
            if isinstance(timer, CountdownTimer):
                if timer.is_expired():
                    timer.stop()
            elif isinstance(timer, Stopwatch):
                timer.stop()

        action = input("Enter 's' to start, 'e' to stop, 'r' to reset, or 'q' to quit: ").lower()
        if action == 's':
            timer.start()
        elif action == 'e':
            timer.stop()
        elif action == 'r':
            timer.reset()
        elif action == 'q':
            break
        else:
            print("Invalid command")

if __name__ == "__main__":
    main()
