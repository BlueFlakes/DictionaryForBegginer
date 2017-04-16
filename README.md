import csv

#   Dictionary for a newbie


#

########--------------------------------------------------------------------------------------------------------########
    # Colors

yellow = "\033[1;33m"
darkblue = "\033[0;34m"
red = "\033[1;31m"
off = "\033[0;0m"
darkcyan = "\033[0;36m"
darkgreen = "\033[0;32m"
darkmagenta = "\033[0;35m"
darkwhite = "\033[0;37m"
darkyellow = "\033[0;33m"
blue = "\033[1;34m"
darkred = "\033[0;31m"
########--------------------------------------------------------------------------------------------------------########

abc_of_dict = {}

########--------------------------------------------------------------------------------------------------------########
    # custom Functions


def zapisz():

    text_file = open("data.csv", "w").close()

    with open('data.csv', 'w') as csvfile:
        fieldnames = ['key', 'FirstDef', 'SecondDef']
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)

        writer.writeheader()
        for key in abc_of_dict:

            writer.writerow({'key': key, 'FirstDef': abc_of_dict[key][0], 'SecondDef': abc_of_dict[key][1]})

#----------------------------------------------------------------------------------------------------------------------#


def print_options():
    print("""
Dictionary for a little programmer:
1) search explanation by appellation
2) add new definition
3) show all appellations alphabetically
4) show available definitions by first letter of appellation
0) exit
        """)

#----------------------------------------------------------------------------------------------------------------------#


def odczytaj():

    with open('data.csv', 'r') as csvfile:

        fieldnames = ['key', 'FirstDef', 'SecondDef']

        reader = csv.DictReader(csvfile)
        list_for_moment = []
        for row in reader:
            list_for_moment.append(row['FirstDef'])
            list_for_moment.append(row['SecondDef'])

            abc_of_dict[(row['key'])] = list_for_moment

            list_for_moment = []


########--------------------------------------------------------------------------------------------------------########

try:
    odczytaj()
except:
    print(darkyellow + "Our software couldn't find a file which contain data. So default values got loaded." + off)
    text_file = open("data.csv", "w").close()

storage = []

for key in abc_of_dict:
    storage.append(key.lower())

running = ""

########--------------------------------------------------------------------------------------------------------########
                                            #### Begin main body ####
#
while running != "stop":

    print_options()

    choice = -1
    while choice not in [0, 1, 2, 3, 4]:
        choice = input(darkcyan + "Tell me your choice: " + off)
        if choice.isdigit():
            choice = int(choice)

        if choice not in [0, 1, 2, 3, 4]:
            print()
            print(red + "Wrong option you choose." + off + darkwhite + " Please try to choose again." + off)
            print_options()

    print()

#----------------------------------------------------------------------------------------------------------------------#

    if choice == 1:

        k = 1
        while k != 0:
            appelation = input("So about which thing u want to know more?: ")

            appel_copy = appelation.replace(" ", "")

            if appel_copy.isalpha():
                k = 0
            else:
                print("Your appelation contain something other than letters! try again.\n")

        appelation = appelation.lower()
        if appelation in storage:

            print(yellow + appelation + " is: " + off + abc_of_dict[appelation][0])
            print(yellow + "The source is:" + off, abc_of_dict[appelation][1])

        else:
            print(darkblue + appelation, "doesn't exist in our dictionary." + off)

#----------------------------------------------------------------------------------------------------------------------#

    elif choice == 2:

        x = 1
        add_new_appel = ""
        while x != 0:
            add_new_appel = input("Key name: ").lower()

            appel_copy = add_new_appel.replace(" ", "")

            if appel_copy.isalpha() and add_new_appel not in storage:
                x = 0
            elif add_new_appel in storage:
                print(add_new_appel+" already exists in our dictionary.\n")
            else:
                print("Error something goes wrong!\n")

        add_new_definition = input("Definition for the appellation: ")
        add_new_source = input("Source of the definition: ")
        added = add_new_definition.capitalize() + "," + add_new_source
        added = added.split(",")

        abc_of_dict[add_new_appel] = added
        storage.append(add_new_appel)

#----------------------------------------------------------------------------------------------------------------------#

    elif choice == 3:

        if len(storage) > 0:

            storage = sorted(storage)
            first_letter = storage[0][0]
            print(first_letter + ":")

            for i in range(len(storage)):  # Print Sorted list by first Letter

                if first_letter != storage[i][0]:
                    first_letter = storage[i][0]

                    print("\n", end="")
                    print(first_letter + ":")

                print(storage[i].capitalize())
        else:
            print("Null elements in dictionary.")
#----------------------------------------------------------------------------------------------------------------------#

    elif choice == 4:

        k = 1
        while k != 0:

            appelation = input("Please choose first letter: ")

            if appelation.isalpha() and len(appelation) == 1:
                k = 0
            else:
                print(red + "Something goes wrong! Error 404.\n" + off)

        print()

        for i in range(len(storage)):
            if appelation.lower() in storage[i][0].lower():
                print(storage[i].capitalize())

#----------------------------------------------------------------------------------------------------------------------#

    elif choice == 0:
        print("Thank you for using our software, see you again someday :)")
        running = "stop"

        zapisz()

#
                                            #### End of main body ####
########--------------------------------------------------------------------------------------------------------########
