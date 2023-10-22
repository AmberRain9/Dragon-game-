# Dragon-game-
A short directional game, guiding you through rooms to collect items and in time defeat dragon. 
import random

def show_instructions():
    # Display game instructions
    print("Text-Based Adventure Game")
    print("Collect all items to win the game and avoid the dragon!")
    print("Commands:")
    print("- 'go North', 'go South', 'go East', 'go West' to move between rooms")
    print("- 'get [key]' to collect an item")
    print("- 'exit' to quit the game")

def show_status(current_room, inventory, items_in_room):
    # Display the player's current room, inventory, and items in the room
    print(f"You are in the {current_room}")
    print(f"Inventory: {', '.join(inventory)}")
    if items_in_room:
        print(f"You see: {', '.join(items_in_room)}")
    else:
        print("The room is empty.")
    print("----------------------------")

# Define the Dictionary for Rooms and Items
rooms = {
    'Great Hall': {'North': 'Bedroom', 'South': 'Library', 'item': 'Sword', 'items': ['Key', 'Chest']},
    'Bedroom': {'South': 'Great Hall', 'East': 'Cellar', 'item': 'Key'},
    'Cellar': {'West': 'Bedroom', 'items': ['Dragon']},
    'Library': {'North': 'Great Hall', 'item': 'Book'},
}

# Initialize player and dragon health
player_hp = 100
dragon_hp = 100

def main():
    current_room = 'Great Hall'
    inventory = []

    while True:
        show_instructions()
        show_status(current_room, inventory, rooms[current_room].get('items', []))

        command = input("Enter your move: ").strip().lower()

        if command == 'exit':
            print("Thanks for playing the game. Hope you enjoyed it.")
            break
        elif command.startswith('go '):
            direction = command.split()[1].capitalize()
            if direction in rooms[current_room]:
                current_room = rooms[current_room][direction]
                # Check if the current room is the cellar and initiate the dragon battle
                if current_room == 'Cellar':
                    print("You have entered the Cellar. Get ready to face the dragon!")
                    dragon_battle()
            else:
                print("You cannot go in that direction.")
        elif command.startswith('get '):
            item_name = command.split(' ', 1)[1].title()
            if 'items' in rooms[current_room] and item_name in rooms[current_room]['items']:
                inventory.append(item_name)
                rooms[current_room]['items'].remove(item_name)
                print(f"You have collected {item_name}.")
            else:
                print(f"There is no {item_name} in this room.")
        elif command == 'place key in chest':
            if 'Key' in inventory and 'Chest' in inventory:
                inventory.remove('Key')
                inventory.remove('Chest')
                inventory.append('Key in Chest')
                print("You've placed the key in the chest.")
            else:
                print("You need both the key and the chest to perform this action.")
        else:
            print("Invalid command. Please enter a valid command.")

def dragon_battle():
    global player_hp
    global dragon_hp

    while player_hp > 0 and dragon_hp > 0:
        print(f"Player HP: {player_hp}  Dragon HP: {dragon_hp}")
        command = input("Enter your move: ").strip().lower()

        if command == 'attack':
            # Implement player's attack logic
            player_damage = 20  # Adjust this based on your game balance
            dragon_hp -= player_damage
            print(f"You dealt {player_damage} damage to the dragon.")

            # Dragon's turn to attack
            dragon_damage = random.randint(10, 30)  # Random damage by the dragon
            player_hp -= dragon_damage
            print(f"The dragon attacks you, dealing {dragon_damage} damage.")
        elif command == 'defend':
            # Implement defend logic
            pass
        elif command == 'use potion':
            # Implement potion usage logic
            pass
        else:
            print("Invalid command. Try again.")

if __name__ == "__main__":
    main()
