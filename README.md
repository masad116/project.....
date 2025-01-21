import tkinter as tk 
from tkinter import messagebox 
import random 
 
# Sudoku puzzle generator using backtracking algorithm 
def generate_sudoku(): 
    def is_valid(board, row, col, num): 
        for i in range(9): 
            if board[row][i] == num or board[i][col] == num: 
                return False 
        start_row, start_col = 3 * (row // 3), 3 * (col // 3) 
        for i in range(3): 
            for j in range(3): 
                if board[start_row + i][start_col + j] == num: 
                    return False 
        return True 
 
    def solve(board): 
        for row in range(9): 
            for col in range(9): 
                if board[row][col] == 0: 
                    for num in range(1, 10): 
                        if is_valid(board, row, col, num): 
                            board[row][col] = num 
                            if solve(board): 
                                return True 
                            board[row][col] = 0 
                    return False 
        return True 
 
    # Generate a full solved Sudoku board 
    board = [[0 for _ in range(9)] for _ in range(9)] 
    solve(board) 
 
    # Remove numbers to create a puzzle 
    for _ in range(random.randint(35, 50)):  # Number of cells to remove 
        row, col = random.randint(0, 8), random.randint(0, 8) 
        board[row][col] = 0 
 
    return board 
 
# Create a GUI for the Sudoku game 
class SudokuGame: 
    def init(self, root): 
        self.root = root 
        self.root.title("Sudoku Game") 
 
        self.board = generate_sudoku() 
        self.entries = [[None for _ in range(9)] for _ in range(9)] 
 
        self.create_grid() 
        self.create_buttons() 
 
    def create_grid(self): 
        for row in range(9): 
            for col in range(9): 
                entry = tk.Entry(self.root, width=5, font=('Arial', 18), justify='center') 
                entry.grid(row=row, column=col, padx=5, pady=5) 
                self.entries[row][col] = entry 
                if self.board[row][col] != 0: 
                    entry.insert(tk.END, str(self.board[row][col])) 
                    entry.config(state="disabled")  # Disable pre-filled cells 
 
    def create_buttons(self): 
        check_button = tk.Button(self.root, text="Check Solution", font=('Arial', 14), 
command=self.check_solution) 
        check_button.grid(row=9, column=0, columnspan=9) 
 
    def check_solution(self): 
        for row in range(9): 
            for col in range(9): 
                user_input = self.entries[row][col].get() 
                if user_input: 
                    if not user_input.isdigit() or int(user_input) != self.board[row][col]: 
                        messagebox.showinfo("Incorrect", f"Wrong value at row {row+1}, column 
{col+1}") 
                        return 
        messagebox.showinfo("Correct", "Congratulations! Your solution is correct!") 
 
# Initialize the Tkinter window 
root = tk.Tk() 
game = SudokuGame(root) 
root.mainloop()
