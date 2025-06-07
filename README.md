import tkinter as tk
from tkinter import messagebox
import winsound  # For playing sounds (Windows only)

class TicTacToe:
    def __init__(self, root):
        self.root = root
        self.root.title("Tic-Tac-Toe")
        self.root.configure(bg="#f0f0f0")

        self.current_player = "X"
        self.board = [""] * 9
        self.buttons = []

        self.score_x = 0
        self.score_o = 0

        self.title_label = tk.Label(root, text="Tic-Tac-Toe Game", font=("Arial", 20, "bold"), bg="#f0f0f0", fg="#333")
        self.title_label.grid(row=0, column=0, columnspan=3, pady=10)

        self.score_label = tk.Label(root, text=self.get_score_text(), font=("Arial", 14), bg="#f0f0f0", fg="#555")
        self.score_label.grid(row=1, column=0, columnspan=3)

        self.status_label = tk.Label(root, text=f"Player {self.current_player}'s turn", font=("Arial", 12), bg="#f0f0f0", fg="#007acc")
        self.status_label.grid(row=2, column=0, columnspan=3, pady=(0, 10))

        for i in range(9):
            button = tk.Button(root, text="", font=("Arial", 24), width=5, height=2,
                               bg="#fff", activebackground="#ddd",
                               command=lambda i=i: self.button_clicked(i))
            button.grid(row=(i // 3) + 3, column=i % 3, padx=5, pady=5)
            self.buttons.append(button)

        self.restart_button = tk.Button(root, text="Restart Game", font=("Arial", 12, "bold"),
                                        bg="#007acc", fg="white", command=self.reset_game)
        self.restart_button.grid(row=6, column=0, columnspan=3, pady=10)

    def get_score_text(self):
        return f"Score - X: {self.score_x}   O: {self.score_o}"

    def play_sound(self, event="move"):
        if event == "move":
            winsound.Beep(600, 100)  # Short beep
        elif event == "win":
            winsound.Beep(900, 300)  # Higher beep
        elif event == "draw":
            winsound.Beep(400, 200)  # Lower beep

    def button_clicked(self, index):
        if self.board[index] == "":
            self.board[index] = self.current_player
            self.buttons[index].config(
                text=self.current_player,
                fg="blue" if self.current_player == "X" else "red"
            )
            self.play_sound("move")

            if self.check_winner():
                self.status_label.config(text=f"Player {self.current_player} wins!")
                self.play_sound("win")
                messagebox.showinfo("Game Over", f"Player {self.current_player} wins!")
                if self.current_player == "X":
                    self.score_x += 1
                else:
                    self.score_o += 1
                self.score_label.config(text=self.get_score_text())
                self.reset_board()
            elif "" not in self.board:
                self.status_label.config(text="It's a draw!")
                self.play_sound("draw")
                messagebox.showinfo("Game Over", "It's a draw!")
                self.reset_board()
            else:
                self.current_player = "O" if self.current_player == "X" else "X"
                self.status_label.config(text=f"Player {self.current_player}'s turn")

    def check_winner(self):
        combos = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8],
            [0, 3, 6], [1, 4, 7], [2, 5, 8],
            [0, 4, 8], [2, 4, 6]
        ]
        for combo in combos:
            a, b, c = combo
            if self.board[a] == self.board[b] == self.board[c] != "":
                self.buttons[a].config(bg="#aaffaa")
                self.buttons[b].config(bg="#aaffaa")
                self.buttons[c].config(bg="#aaffaa")
                return True
        return False

    def reset_board(self):
        self.board = [""] * 9
        for button in self.buttons:
            button.config(text="", bg="#fff")
        self.current_player = "X"
        self.status_label.config(text="Player X's turn")

    def reset_game(self):
        self.reset_board()
        self.score_x = 0
        self.score_o = 0
        self.score_label.config(text=self.get_score_text())

# Run the game
if __name__ == "__main__":
    window = tk.Tk()
    game = TicTacToe(window)
    window.mainloop()
