import random

class Game2048:
    def __init__(self, size=3):
        self.size = size
        self.board = [[0] * size for _ in range(size)]
        self.add_new_tile()
        self.add_new_tile()

    def add_new_tile(self):
        empty_tiles = [(i, j) for i in range(self.size) for j in range(self.size) if self.board[i][j] == 0]
        if not empty_tiles:
            return
        i, j = random.choice(empty_tiles)
        self.board[i][j] = 2 if random.random() < 0.9 else 4

    def slide_row_left(self, row):
        new_row = [num for num in row if num != 0]
        new_row += [0] * (self.size - len(new_row))
        for i in range(len(new_row) - 1):
            if new_row[i] == new_row[i + 1] and new_row[i] != 0:
                new_row[i] *= 2
                new_row[i + 1] = 0
        new_row = [num for num in new_row if num != 0]
        new_row += [0] * (self.size - len(new_row))
        return new_row

    def move_left(self):
        new_board = []
        for row in self.board:
            new_board.append(self.slide_row_left(row))
        self.board = new_board
        self.add_new_tile()

    def move_right(self):
        self.board = [row[::-1] for row in self.board]
        self.move_left()
        self.board = [row[::-1] for row in self.board]

    def move_up(self):
        self.board = [list(row) for row in zip(*self.board)]
        self.move_left()
        self.board = [list(row) for row in zip(*self.board)]

    def move_down(self):
        self.board = [list(row) for row in zip(*self.board)]
        self.move_right()
        self.board = [list(row) for row in zip(*self.board)]

    def print_board(self):
        for row in self.board:
            print("\t".join(map(str, row)))
        print()

def main():
    game = Game2048()
    game.print_board()
    while True:
        move = input("Enter move (w/a/s/d): ").strip().lower()
        if move == 'a':
            game.move_left()
        elif move == 'd':
            game.move_right()
        elif move == 'w':
            game.move_up()
        elif move == 's':
            game.move_down()
        else:
            print("Invalid move! Use w/a/s/d keys.")
            continue
        game.print_board()

if __name__ == "__main__":
    main()
