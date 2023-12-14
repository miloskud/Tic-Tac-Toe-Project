## Description
On one fateful day, during a boring math class, with the whiteboards out. Me and one of my friends decided to change up the classic game of Tic Tac Toe. We decided play with a 5x5 board, requiring a 4 in a row sequence to win. The game was essentially tic tac toe, except it has significantly more tricks and traps that can be used to garuntee a win. This makes it more engaging and challenging than traditional tic tac toe. I had so much fun with this game during math class, that I decided to turn it into my final project for Pi's and Python. It could the game into a unique and repetable experience.
## Materials Needed
- 128×64 OLED display module (SSD1306)
- Raspberry Pi
- Breadboard
- KeyeStudios Raspberry pi buttons x3
- Wires
## Additional Notes:
- The buttons are designed to be held down, and are not meant to produce a single output. Because of this, you may need to press the buttons one or two extra times to produce the desired output, or just hold it down for a little bit longer than you normally would.
- The game is 5x5 Tic-Tac-Toe. In order to win, you need to have 4 in a row in any direction.
- Every odd round is controlled by the o, and every even round is controlled by the x. It automatically switches which one is placed after each round.
- The button layout is:
S  R
D
- The top left button is the selection button
- The top right button moves the selector right.
- The bottom button moves the selector down.
- You can to return to the start of the row or column by continuing to press the button in the desired direction, which will move the selector back to the start of the row or column


# IMPORTANT!!!! :
In order to use the font, you must download the "rainyhearts" ttf file from 
http://www.dafont.com/bitmap.bhp
and the .ttf file must be contained in the same folder as the python file.
The program will not work without this.
## Wiring:
![Wiring](https://github.com/miloskud/Tic-Tac-Toe-Project/assets/24467735/f73ec7a3-47ad-4ee4-992e-7a9fe65cb442)

A picture of the wiring is in github.
The bottom line is:
- Screen connected to SDA, SCL, 3.3V, and GND
- Each Button connected to 5.0V, and GND
- Up connected to GPIO 22
- Down connected to GPIO 23
- Selection connected to GPIO 27

## Code:

```python

import Adafruit_GPIO.SPI as SPI
import Adafruit_SSD1306

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont
import RPi.GPIO as GPIO

#Setup
GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)
GPIO.setup(27, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
GPIO.setup(22, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
GPIO.setup(23, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)

# Raspberry Pi pin configuration:
RST = None     # on the PiOLED this pin isnt used
# Note the following are only used with SPI:
DC = 23
SPI_PORT = 0
SPI_DEVICE = 0


disp = Adafruit_SSD1306.SSD1306_128_32(rst=RST)



# Initialize library.
disp.begin()

# Clear display.
disp.clear()
disp.display()

# Create blank image for drawing.
# Make sure to create image with mode '1' for 1-bit color.
width = disp.width
height = disp.height
image = Image.new('1', (width, height))

# Get drawing object to draw on image.
draw = ImageDraw.Draw(image)

# Draw a black filled box to clear the image.
draw.rectangle((0,0,width,height), outline=0, fill=0)

# Draw some shapes.
# First define some constants to allow easy resizing of shapes.
padding = -1
top = padding
bottom = height-padding
# Move left to right keeping track of the current x position for drawing shapes.
x = 0


# Load default font.
font = ImageFont.load_default()
#Array of possible choices for play
board = [[" - ", " - ", " - ", " - ", " - "],
        [" - ", " - ", " - ", " - ", " - "],
        [" - ", " - ", " - ", " - ", " - "],
        [" - ", " - ", " - ", " - ", " - "],
        [" - ", " - ", " - ", " - ", " - "]]
#Check each possible win condition on the grid.
def checkWin():
    for row in range(5):
        if((board[row][0] == " x " and board[row][1] == " x " and board[row][2] == " x " and board[row][3] == " x ") or 
           (board[row][1] == " x " and board[row][2] == " x " and board[row][3] == " x " and board[row][4] == " x ")):
            print("X VICTORY!")
            return "x"
        elif((board[row][0] == " o " and board[row][1] == " o " and board[row][2] == " o " and board[row][3] == " o ") or 
            (board[row][1] == " o " and board[row][2] == " o " and board[row][3] == " o " and board[row][4] == " o ")):
            print("O VICTORY!")
            return "o"
        else:
            continue

    for column in range(5):
        if((board[0][column] == " x " and board[1][column] == " x " and board[2][column] == " x " and board[3][column] == " x ") or 
           (board[1][column] == " x " and board[2][column] == " x " and board[3][column] == " x " and board[4][column] == " x ")):
            print("X VICTORY!")
            return "x"
        if((board[0][column] == " o " and board[1][column] == " o " and board[2][column] == " o " and board[3][column] == " o ") or 
           (board[1][column] == " o " and board[2][column] == " o " and board[3][column] == " o " and board[4][column] == " o ")):
            print("O VICTORY!")
            return "o"
        else:
            continue
    for group in range(1):
        if((board[0+group][0+group] == " x " and board[1+group][1+group] == " x " and board[2+group][2+group] == " x " and board[3+group][3+group] == " x ")):
            print("X VICTORY!")
            return "x"
        if((board[0][0-group] == " x " and board[1][1-group] == " x " and board[2][2-group] == " x " and board[3][3-group] == " x ")):
            print("X VICTORY!")
            return "x"
        if((board[0][0+group] == " x " and board[1][1+group] == " x " and board[2][2+group] == " x " and board[3][3+group] == " x ")):
            print("X VICTORY!")
            return "x"
        if((board[1][0] == " x " and board[2][1] == " x " and board[3][2] == " x " and board[4][3] == " x ")):
            print("X VICTORY!")
            return "x"
        if((board[0][1] == " x " and board[1][2] == " x " and board[2][3] == " x " and board[3][4] == " x ")):
            print("X VICTORY!")
            return "x"
        if((board[1][2] == " x " and board[2][3] == " x " and board[3][4] == " x " and board[4][5] == " x ")):
            print("X VICTORY!")
            return "x"
        if((board[1][0] == " x " and board[2][1] == " x " and board[3][2] == " x " and board[4][3] == " x ")):
            print("X VICTORY!")
            return "x"
        if((board[0+group][0+group] == " o " and board[1+group][1+group] == " o " and board[2+group][2+group] == " o " and board[3+group][3+group] == " o ")):
            print("O VICTORY!")
            return "o"
        if((board[0][0-group] == " o " and board[1][1-group] == " o " and board[2][2-group] == " o " and board[3][3-group] == " o ")):
            print("O VICTORY!")
            return "o"
        if((board[0][0+group] == " o " and board[1][1+group] == " o " and board[2][2+group] == " o " and board[3][3+group] == " o ")):
            print("O VICTORY!")
            return "o"
        if((board[1][0] == " o " and board[2][1] == " o " and board[3][2] == " o " and board[4][3] == " o ")):
            print("O VICTORY!")
            return "o"
        if((board[0][1] == " o " and board[1][2] == " o " and board[2][3] == " o " and board[3][4] == " o ")):
            print("O VICTORY!")
            return "o"
        if((board[1][2] == " o " and board[2][3] == " o " and board[3][4] == " o " and board[4][5] == " o ")):
            print("O VICTORY!")
            return "o"
        if((board[1][0] == " o " and board[2][1] == " o " and board[3][2] == " o " and board[4][3] == " o ")):
            print("O VICTORY!")
            return "o"
        else:
            continue

    for group in range(1):
        if((board[4-group][0-group] == " x " and board[3-group][1-group] == " x " and board[2-group][2-group] == " x " and board[1-group][3-group] == " x ")):
            print("X VICTORY!")
            return "o"
        if((board[4-group][0-group] == " o " and board[3-group][1-group] == " o " and board[2-group][2-group] == " o " and board[1-group][3-group] == " o ")):
            print("O VICTORY!")
            return "o"
        else:
            continue
    for group in range(1):
        if((board[4][0-group] == " x " and board[3][1-group] == " x " and board[2][2-group] == " x " and board[1][3-group] == " x ")):
            print("X VICTORY!")
            return "o"
        if((board[4][0-group] == " o " and board[3][1-group] == " o " and board[2][2-group] == " o " and board[1][3-group] == " o ")):
            print("O VICTORY!")
            return "o"
        else:
            continue
    for group in range(1):
        if((board[4-group][0] == " x " and board[3-group][1] == " x " and board[2-group][2] == " x " and board[1-group][3] == " x ")):
            print("X VICTORY!")
            return "o"
        if((board[4-group][0] == " o " and board[3-group][1] == " o " and board[2-group][2] == " o " and board[1-group][3] == " o ")):
            print("O VICTORY!")
            return "o"
        else:
            continue
    return ""

def selectSquare(round):
    
    posx = 1
    posy = 1

    #Move Right (GPIO 22)
    while True:
        if(GPIO.input(22) == GPIO.LOW):
            if(posx < 5):
                posx += 1
            else:
                posx = 1
    #Move Down (GPIO 23)
        elif(GPIO.input(23) == GPIO.LOW):
            if(posy < 5):
                posy += 1
            else:
                posy = 1
    #Coordinate display
        draw.rectangle((90,9,140,25), outline=0, fill=0)
        draw.text((90, 6), "x:" + str(posx) + "," + "y:" + str(posy), font=ImageFont.truetype('rainyhearts.ttf', 18), fill=255) 

        disp.image(image)
        disp.display()
        time.sleep(.1)
    #Select button
        if GPIO.input(27) == GPIO.LOW:
            if(board[posy-1][posx-1] == " - "):
                if(round % 2 == 0):
                    board[posy-1][posx-1] = " x "
                else:
                    board[posy-1][posx-1] = " o "
                break

        continue

def main():
    round = 1
    winner = ""
    while True:
        #Draw a black filled box to clear the image.
        draw.rectangle((0,0,width,height), outline=0, fill=0)
        #Display board
        draw.text((x, top),       board[0][0] + board[0][1] + board[0][2] + board[0][3] + board[0][4],  font=font, fill=255)
        draw.text((x, top+6),     board[1][0] + board[1][1] + board[1][2] + board[1][3] + board[1][4], font=font, fill=255)
        draw.text((x, top+12),    board[2][0] + board[2][1] + board[2][2] + board[2][3] + board[2][4],  font=font, fill=255)
        draw.text((x, top+18),    board[3][0] + board[3][1] + board[3][2] + board[3][3] + board[3][4],  font=font, fill=255)
        draw.text((x, top+24),    board[4][0] + board[4][1] + board[4][2] + board[4][3] + board[4][4],  font=font, fill=255)
        selectSquare(round)

        disp.image(image)
        disp.display()
        time.sleep(.5)

        #Check whether there is a winner (and who it is)
        winner = checkWin()
        if(winner == "x"):
            draw.rectangle((0,0,130,100), outline=0, fill=0)
            draw.text((6, 6), "X WINS!", font=ImageFont.truetype('rainyhearts.ttf', 30), fill=255)
            disp.image(image)
            disp.display()
            time.sleep(.5)
            break
        elif(winner == "o"):
            draw.rectangle((0,0,130,100), outline=0, fill=0)
            draw.text((6, 6), "O WINS!", font=ImageFont.truetype('rainyhearts.ttf', 30), fill=255)
            disp.image(image)
            disp.display()
            time.sleep(.5)
            break
        #Next round (For deciding who's turn it is)
        round += 1


main()




```
