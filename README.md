# Lab1Recursion
'''
Elizabeth Rubio
Lab 1
'''

import hashlib

def hash_with_sha256(str):
    hash_object = hashlib.sha256(str.encode('utf-8'))
    hex_dig = hash_object.hexdigest()
    return hex_dig

#Only reads the file in lines later to be divided into commas
def fileReading():
    textOpener = open('password_file.txt', 'r')
    readFile = textOpener.read().splitlines()
    textOpener.close()
    return readFile

def passwordR(current, unused, length, file):
    #the base case cosiders if unused is 0 or if the length exceeds that
    #of the users input. If they are met then the if statement also checks for
    #the string for the users password match.
    if len(unused) == 0 or len(current) > length:
        printVersion = ''
        for x in range(len(file)):
            charCombined = ''.join(current)
            printVersion = charCombined
            charCombined = charCombined + file[x][1]
            hex_dig = hash_with_sha256(charCombined)
            if hex_dig == file[x][2]:
                print(file[x][0] + "'s password is " + printVersion)
            charCombined = ''
        return
    #the for loop creates the series of premutations which the recursion is
    #moved on by appending the 'item' into a new varaible, 'new_cur'. Rather
    #than removing from the unused list the 'item' will keep moving forward
    #with the for loop and the base case will stop when a new combination
    #is created
    for item in unused:
        new_cur = current.copy()
        new_cur.append(item)
        passwordR(new_cur, unused, length, file)
    

def main():
    textFile = fileReading()
    newFile = []
    #splitting the file from just rows to column
    for row in range(len(textFile)):
        newFile.append(textFile[row].split(','))
    #user input is taken for the length which will then be checked if it fits
    #the appropriate parameters. Otherwise the program ends
    print("Input size of password between :", end = '')
    lenPassword = int(input())
    if lenPassword < 3 or lenPassword > 7:
        print("Invalid size. Run program again.")
        return
    setNumbers = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
    passwordR([], setNumbers, lenPassword-1, newFile)

main()
#hex_dig = hash_with_sha256('1')
