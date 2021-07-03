# python3.8games
# Hangman game

import random


class HangMan(object):

    # Hangman game
    hang = [' +---+', ' |   |', '     |', '     |', '     |', '     |', '=======']

    man = {0: [' 0   |'], 1: [' 0   |', ' |   |'], 2: [' 0   |', '/|   |'], 3: [' 0   |', '/|\\  |'],
           4: [' 0   |', '/|\\  |', '/    |'], 5: [' 0   |', '/|\\  |', '/ \\  |']}

    pics = []

    words = '''ant baboon badger bat bear beaver camel cat clam cobra cougar coyote
crow deer dog donkey duck eagle ferret fox frog goat goose hawk lion lizard llama
mole monkey moose mouse mule newt otter owl panda parrot pigeon python rabbit ram
rat raven rhino salmon seal shark sheep skunk sloth snake spider stork swan tiger
toad trout turkey turtle weasel whale wolf wombat zebra brawl stars sand band'''.split()

    infStr = '_-*\'*-_-*\'*-_-*\'*-_-*\'*-_-*\'*-_-*\'*-_-*\'*-_-*\'*-_-*\'*-_-*\''

    def __init__(self):
        i, j = 2, 0
        self.pics.append(self.hang[:])
        for ls in self.man.values():
            pic, j = self.hang[:], 0
            for m in ls:
                pic[i + j] = m
                j += 1
            self.pics.append(pic)

    def pickWord(self):
        return self.words[random.randint(0, len(self.words) - 1)]

    def printPic(self, idx):
        for line in self.pics[idx]:
            print(line)

    @staticmethod
    def askAndEvaluate(word, result, missed):
        guess = input()
        if guess is None or len(guess) != 1 or (guess in result) or (guess in missed):
            return None, False
        i = 0
        right = guess in word
        for c in word:
            if c == guess:
                result[i] = c
            i += 1
        return guess, right

    def info(self, info):
        len(self.infStr)
        print(self.infStr[:-3])
        print(info)
        print(self.infStr[3:])

    def start(self):
        print('Welcome to Hangman !')
        word = list(self.pickWord())
        result = list('*' * len(word))
        print('The word is: ', result)
        success, i, missed = False, 0, []
        while i < len(self.pics)-1:
            print('Guess the word: ', end='')
            guess, right = self.askAndEvaluate(word, result, missed)
            if guess is None:
                print('You\'ve already entered this character.')
                continue
            print(''.join(result))
            if result == word:
                self.info('Congratulations ! You\'ve just saved a life ! Thank you so much')
                success = True
                break
            if not right:
                missed.append(guess)
                i += 1
            self.printPic(i)
            print('Missed characters: ', missed)

        if not success:
            self.info('The word was \''+''.join(word)+'\' ! You\'ve just killed a man, yo !')


HangMan().start()
"""Memory, puzzle game of number pairs."""

from random import *
from turtle import *

from freegames import path

car = path('car.gif')
tiles = list(range(32)) * 2
state = {'mark': None}
hide = [True] * 64


def square(x, y):
    """Draw white square with black outline at (x, y)."""
    up()
    goto(x, y)
    down()
    color('black', 'white')
    begin_fill()
    for count in range(4):
        forward(50)
        left(90)
    end_fill()


def index(x, y):
    """Convert (x, y) coordinates to tiles index."""
    return int((x + 200) // 50 + ((y + 200) // 50) * 8)


def xy(count):
    """Convert tiles count to (x, y) coordinates."""
    return (count % 8) * 50 - 200, (count // 8) * 50 - 200


def tap(x, y):
    """Update mark and hidden tiles based on tap."""
    spot = index(x, y)
    mark = state['mark']

    if mark is None or mark == spot or tiles[mark] != tiles[spot]:
        state['mark'] = spot
    else:
        hide[spot] = False
        hide[mark] = False
        state['mark'] = None


def draw():
    """Draw image and tiles."""
    clear()
    goto(0, 0)
    shape(car)
    stamp()

    for count in range(64):
        if hide[count]:
            x, y = xy(count)
            square(x, y)

    mark = state['mark']

    if mark is not None and hide[mark]:
        x, y = xy(mark)
        up()
        goto(x + 2, y)
        color('black')
        write(tiles[mark], font=('Arial', 30, 'normal'))

    update()
    ontimer(draw, 100)


shuffle(tiles)


def setup():
    pass


setup()
addshape(car)
hideturtle()
tracer(False)
onscreenclick(tap)
draw()
done()
