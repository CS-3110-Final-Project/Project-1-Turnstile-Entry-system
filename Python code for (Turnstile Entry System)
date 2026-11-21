
# CS 3110 - Formal Languages and Automata
# Turnstile Entry System with Emergency Trigger
#
# Group Members:
# Jacob Lee
# Justin Li
# Cecilia Nguyen
# Hope Gomez

# Five possible states of the turnstile
STATES = (
    "Locked",
    "Unlocked",
    "Entry_Granted",
    "Alarm",
    "Emergency_Unlocked"
)

# Six inputs the system can receive
INPUTS = (
    "coin_inserted",
    "pass_scanned",
    "push_attempt",
    "pass_through",
    "emergency_trigger",
    "reset_emergency"
)

# The turnstile starts locked
START_STATE = "Locked"

# Accepting states defined in the project instructions
ACCEPTING_STATES = {
    "Locked",
    "Entry_Granted",
    "Emergency_Unlocked"
}

# Create the DFA transition table
# Unspecified inputs keep the current state
TRANSITIONS = {
    state: {event: state for event in INPUTS}
    for state in STATES
}

# Valid payment unlocks the turnstile
# Pushing while locked activates the alarm
TRANSITIONS["Locked"].update({
    "coin_inserted": "Unlocked",
    "pass_scanned": "Unlocked",
    "push_attempt": "Alarm"
})

# Passing through an unlocked turnstile grants entry
TRANSITIONS["Unlocked"].update({
    "pass_through": "Entry_Granted",
    "push_attempt": "Entry_Granted"
})

# Reset the alarm and return to Locked
TRANSITIONS["Alarm"]["reset_emergency"] = "Locked"

# Return to Locked after a completed entry
TRANSITIONS["Entry_Granted"]["pass_through"] = "Locked"

# Restore normal operation after an emergency
TRANSITIONS["Emergency_Unlocked"]["reset_emergency"] = "Locked"

# Emergency input overrides any current state
for state in STATES:
    TRANSITIONS[state]["emergency_trigger"] = "Emergency_Unlocked"


# Class that manages the current DFA state
class Turnstile:

    def __init__(self):
        self.state = START_STATE

    # Process an input and update the state
    def handle(self, event):

        if event not in INPUTS:
            raise ValueError("Invalid input: " + str(event))

        previous = self.state
        self.state = TRANSITIONS[previous][event]

        return previous, self.state


# Command-line interface for the user
def main():

    machine = Turnstile()

    print("Turnstile Entry System")
    print("CS 3110 Group Project")

    # Continue running until the user exits
    while True:

        print("\nCurrent State:", machine.state)
        print("Accepting State:",
              machine.state in ACCEPTING_STATES)

        # Display the available inputs
        print("\nSelect an action:")

        for i, event in enumerate(INPUTS, start=1):
            print(f"{i}. {event}")

        print("0. Exit")

        choice = input("\nEnter choice: ").strip()

        # Exit the program
        if choice == "0":
            print("Exiting system.")
            break

        # Check for invalid menu inputs
        if not choice.isdigit():
            print("Invalid input. Enter a number.")
            continue

        choice = int(choice)

        if choice < 1 or choice > len(INPUTS):
            print("Invalid selection.")
            continue

        # Convert the menu choice into a DFA input
        event = INPUTS[choice - 1]

        # Perform the state transition
        previous, current = machine.handle(event)

        print("\nTransition:")
        print(previous, "-->", current)

        # Display the result of the transition
        if current == "Alarm":
            print("ALARM! Unauthorized entry attempted.")

        elif current == "Emergency_Unlocked":
            print("EMERGENCY! Turnstile unlocked.")

        elif current == "Entry_Granted":
            print("Entry granted successfully.")

        elif current == "Unlocked":
            print("Turnstile unlocked.")

        elif current == "Locked":
            print("Turnstile is locked.")


# Start the program
if __name__ == "__main__":
    main()
