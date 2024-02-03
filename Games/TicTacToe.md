## Tic Tac Toe

```go

func tictactoe(state string) string {
	stateArray := strings.Split(state, ",")

	if len(stateArray) != 9 {
		return "Error"
	}

	board := [3][3]string{}
	for i := 0; i < len(stateArray); i++ {
		if !isValidCell(stateArray[i]) {
			return "Error"
		}
		board[i/3][i%3] = strings.ToUpper(stateArray[i])
	}

	if checkRow(board, "X") || checkCol(board, "X") || checkDiagonal(board, "X") {
		return "X Winner"
	}

	if checkRow(board, "O") || checkCol(board, "O") || checkDiagonal(board, "O") {
		return "O Winner"
	}

	for _, cell := range stateArray {
		if cell == "" {
			return "Incomplete"
		}
	}
	return "Tie"
}

func isValidCell(cell string) bool {
	return cell == "X" || cell == "O" || cell == "" || cell == "x" || cell == "o"
}

func checkRow(board [3][3]string, player string) bool {
	for i := 0; i < 3; i++ {
		if board[i][0] == player && board[i][1] == player && board[i][2] == player {
			return true
		}
	}
	return false
}
func checkCol(board [3][3]string, player string) bool {
	for i := 0; i < 3; i++ {
		if board[0][i] == player && board[1][i] == player && board[2][i] == player {
			return true
		}
	}
	return false
}
func checkDiagonal(board [3][3]string, player string) bool {
	if board[0][0] == player && board[1][1] == player && board[2][2] == player {
		return true
	}
	if board[2][0] == player && board[1][1] == player && board[0][2] == player {
		return true
	}
	return false
}

```