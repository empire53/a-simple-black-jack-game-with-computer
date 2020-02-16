# a-simple-black-jack-game-with-computer
In this section we will learn a simple black jack game where a player play with computer(dealer).
import random, copy, dealer

# this section checks if the player would like to play with betting enabled or not.  If they choose yes,
# then 'betting' = True, and a set of 'if betting = True:' statements throughout the program turn on
print("Would you like to turn on betting?")
betting_on = input("Type Yes if so:")
print("")
if betting_on == 'Yes':
  betting = True
  player_wallet = 100
else:
  betting = False

# game setup
keep_playing = True
while keep_playing:
  # this block here copies a new deck into the playing_deck variable, and resets the other playing variables
  playing_deck = copy.copy(dealer.the_deck)
  player_hand = []
  player_score = 0
  dealer_hand = []
  dealer_score = 0

  # this block draws the player's starting hand, and tells them what they have
  player_hand.append(dealer.draw_card(playing_deck))
  player_hand.append(dealer.draw_card(playing_deck))
  player_score = dealer.compute_score(player_hand)
  print("Starting hand: " + dealer.cards_in_hand(player_hand))
  print("Starting score: " + str(player_score))
  if betting == True:
    print("Player Credits: " + str(player_wallet))
  print("")
 
 # here the player places their bet
  player_bet = 0
  if betting == True:
      while player_bet == 0:
        try:
          player_bet = int(input("How many credits would you like to wager?"))
          if player_bet > player_wallet:
            print("You don't have that many credits to wager, try again!")
            player_bet = 0
        except:
          print("That's not a number, try again!")
          player_bet = 0
  print("")
  
  # this is the start of the proper game loop
  in_game = True
  player_wins = None
  while in_game:
    print("Would you like to draw a card?")
    keep_going = input("Type 'hit' to draw, type 'stay' to end.")
    print('')
    # if the player chooses to hit...
    if keep_going == "hit":
      player_hand.append(dealer.draw_card(playing_deck))
      player_score = dealer.compute_score(player_hand)
      if player_score > 21:
        print("Current hand: " + dealer.cards_in_hand(player_hand))
        print("Current score: " + str(player_score))
        print('')
        print("Oh no!  You went bust!")
        player_wins = False
        in_game = False
      elif len(player_hand) > 4:
        print("Wow!  You got a 'Five Card Charlie'!  You Win!")
        player_wins = True
        in_game = False
      else:
        print("Current hand: " + dealer.cards_in_hand(player_hand))
        print("Current score: " + str(player_score) + "\n")
    # if the player chooses to stay, the dealer then draws and plays
    elif keep_going == "stay":
      print("Now the dealer will draw.")
      dealer_hand.append(dealer.draw_card(playing_deck))
      dealer_hand.append(dealer.draw_card(playing_deck))
      dealer_score = dealer.compute_score(dealer_hand)
      while dealer_score < 16:
        dealer_hand.append(dealer.draw_card(playing_deck))
        dealer_score = dealer.compute_score(dealer_hand)
      print("The dealer's hand is: " + dealer.cards_in_hand(dealer_hand))
      print("The dealer's score is: " + str(dealer_score))
      print('')
      if dealer_score > 21:
        print("The dealer went bust!  You win!")
        player_wins = True
        in_game = False
      elif len(dealer_hand) > 4:
        print("The dealder got a 'Five Card Charlie'.  You lose!")
        player_wins = False
        in_game = False
      elif dealer_score >= player_score:
        print("The dealer beat you!  You lose!")
        player_wins = False
        in_game = False
      else:
        print("You beat the dealer!  You win!")
        player_wins = True
        in_game = False
  # here the player's bet gets resolved
  if betting == True:
    if player_wins == True:
      player_wallet = player_wallet + player_bet
    elif player_wins == False:
      player_wallet = player_wallet - player_bet
    if player_wallet < 1:
      print("You've run out of credits!\nGame Over!")
      break
  # this checks if the player wants to play again
  check_continue = input("\nPress Enter if you would like to keep playing; type 'No' to stop.").lower()
  if check_continue == "No" or "no" :
    print("Thanks for playing!")
    keep_playing = False  
  else:
    print('\n````` NEW GAME `````\n')

#create a new file in the same file and attach it with t upper one#
import random

#This instantiation of a 52 card deck is a set of tuples, each containing a card name string at index 0
#and a card value at index 1.  The .png file names at index 2 are from an attempt to create a graphical
#representation of the cards that were in play, which did not pan out.
the_deck = [("Ace of Hearts", 11,), ("2 of Hearts", 2, ), \
("3 of Hearts", 3, ), ("4 of Hearts", 4, ), \
("5 of Hearts", 5, ), ("6 of Hearts", 6, ), \
("7 of Hearts", 7, ), ("8 of Hearts", 8, ), \
("9 of Hearts", 9, ), ("10 of Hearts", 10,  ), \
("Jack of Hearts", 10, ), ("Queen of Hearts", 10, ), \
("King of Hearts", 10, ), ("Ace of Spades", 11, ), \
("2 of Spades", 2,), ("3 of Spades", 3, ), \
("4 of Spades", 4, ), ("5 of Spades", 5, ), \
("6 of Spades", 6, ), ("7 of Spades", 7, ), \
("8 of Spades", 8, ), ("9 of Spades", 9, ), \
("10 of Spades", 10, ), ("Jack of Spades", 10, ), \
("Queen of Spades", 10, ), ("King of Spades", 10, ), \
("Ace of Diamonds", 11, ), ("2 of Diamonds", 2, ), \
("3 of Diamonds", 3,), ('4 of Diamonds', 4, ), \
("5 of Diamonds", 5, ), ("6 of Diamonds", 6, ), \
("7 of Diamonds", 7, ), ("8 of Diamonds", 8, ), \
("9 of Diamonds", 9, ), ("10 of Diamonds", 10, ), \
("Jack of Diamonds", 10, ), ("Queen of Diamonds", 10, ), \
("King of Diamonds", 10, ), ("Ace of Clubs", 11, ), \
("2 of Clubs", 2, ), ("3 of Clubs", 3, ), \
("4 of Clubs", 4, ), ("5 of Clubs", 5, ), \
("6 of Clubs", 6, ), ("7 of Clubs", 7, ), \
("8 of Clubs", 8, ), ("9 of Clubs", 9, ), \
("10 of Clubs", 10, ), ("Jack of Clubs", 10, ), \
("Queen of Clubs", 10, ), ("King of Clubs", 10, )]

# when given a list, this function will choose an item in the list, remove it from the list, and return it.
# it is intended to choose a card out of a list representing a deck, remove it from the deck list,
# and return it to a list representing a player's hand.
def draw_card(x):
  drawn_card = random.choice(x)
  x.remove(drawn_card)
  return drawn_card

# this function takes the intergers from the card tuples and puts them into a list of values.
# it then checks to see if the sum of the values would be over 21.  If they are, it checks if there
# are any 11s in the value list, representing aces.  If so, it replaces one of the 11s with a 1, and
# checks the sum again.  It continues this until the value is less than 21, or there are no more aces
# in the list.  Then, it returns the final total for the values.  This is then the value of a player's hand
# for the purposes of scoring.
def compute_score(x):
  total = 0
  value_set = []
  for i in x:
    value_set.append(i[1])  
  total = sum(value_set)
  ace_check = True
  while ace_check == True:
    if total > 21 and 11 in value_set:
      value_set.remove(11)
      value_set.append(1)
      total = sum(value_set)
    else:
      ace_check = False
  return total

# this function is not as necessary for playing the game, but helps with readability.  It takes the card
# tuples in a hand list, and extracts the card name strings.  These are then concatenated, and then returned
def cards_in_hand(x):
  hand = []
  for i in x:
    hand.append(i[0])
  handstring = hand[0]
  for i in hand[1:]:
    handstring = handstring + ', ' + i
  return handstring
