# CODE-ALPHA-TASK-1-HANGMAN-GAME
#HANGMAN GAME
import tkinter as tk
from tkinter import messagebox
import random

class HangmanGUI:
    def __init__(self, master):
        self.master = master
        self.master.title("Hangman")
        self.master.geometry("600x600")  # Increased canvas size
        self.canvas = tk.Canvas(master, width=400, height=400)  # Increased canvas size
        self.canvas.pack()

        self.word = self.choose_word()
        self.display_word = ["_"] * len(self.word)
        self.guessed_letters = []
        self.max_attempts = 6
        self.incorrect_guesses = 0

        self.draw_word()
        self.draw_hangman()

        self.entry = tk.Entry(master)
        self.entry.pack()
        self.entry.focus_set()

        self.guess_button = tk.Button(master, text="Guess", command=self.check_guess)
        self.guess_button.pack()

    def choose_word(self):
        words = ["apple", "banana", "orange", "grape", "pear", "strawberry", "pineapple", "blueberry"]
        return random.choice(words)

    def draw_word(self):
        self.canvas.delete("word")
        word_text = " ".join(self.display_word)
        self.canvas.create_text(200, 50, text=word_text, tags="word", font=("Arial", 20))  # Larger font size

    def draw_hangman(self):
        self.canvas.delete("hangman")
        if self.incorrect_guesses > 0:
            self.canvas.create_line(100, 380, 300, 380, tags="hangman", width=5)  # base
        if self.incorrect_guesses > 1:
            self.canvas.create_line(200, 380, 200, 100, tags="hangman", width=5)  # pole
        if self.incorrect_guesses > 2:
            self.canvas.create_line(200, 100, 350, 100, tags="hangman", width=5)  # top
        if self.incorrect_guesses > 3:
            self.canvas.create_line(350, 100, 350, 150, tags="hangman", width=5)  # rope
        if self.incorrect_guesses > 4:
            self.canvas.create_oval(320, 150, 380, 210, tags="hangman", width=5)  # head
        if self.incorrect_guesses > 5:
            self.canvas.create_line(350, 210, 350, 300, tags="hangman", width=5)  # body
            self.canvas.create_line(350, 240, 300, 280, tags="hangman", width=5)  # left arm
            self.canvas.create_line(350, 240, 400, 280, tags="hangman", width=5)  # right arm
            self.canvas.create_line(350, 300, 300, 360, tags="hangman", width=5)  # left leg
            self.canvas.create_line(350, 300, 400, 360, tags="hangman", width=5)  # right leg

    def check_guess(self):
        guess = self.entry.get().lower()
        self.entry.delete(0, tk.END)

        if len(guess) != 1 or not guess.isalpha():
            messagebox.showinfo("Invalid Input", "Please enter a single letter.")
            return

        if guess in self.guessed_letters:
            messagebox.showinfo("Already Guessed", "You've already guessed that letter.")
            return

        self.guessed_letters.append(guess)

        if guess in self.word:
            for i, letter in enumerate(self.word):
                if letter == guess:
                    self.display_word[i] = guess
            self.draw_word()
        else:
            self.incorrect_guesses += 1
            self.draw_hangman()

        if "_" not in self.display_word:
            messagebox.showinfo("Congratulations!", f"You guessed the word: {''.join(self.display_word)}")
            self.master.destroy()
        elif self.incorrect_guesses == self.max_attempts:
            messagebox.showinfo("Game Over", f"Sorry, you're out of attempts. The word was: {self.word}")
            self.master.destroy()

def main():
    root = tk.Tk()
    hangman_game = HangmanGUI(root)
    root.mainloop()

if __name__ == "__main__":
    main()
