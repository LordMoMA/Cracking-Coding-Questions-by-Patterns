The problem is about a game of Rock, Paper, Scissors. In this game, each player simultaneously forms one of three possible gestures: Rock (R), Paper (P), or Scissors (S). The winner is determined by the rules: Rock crushes Scissors, Scissors cuts Paper, and Paper covers Rock. If both players make the same gesture, it's a tie.

The function Solution(G string) int is designed to calculate the maximum number of points a player can score if they were to use only one gesture (either all Rock, all Paper, or all Scissors) throughout a series of games. The games are represented by a string G, where each character represents the gesture of the opponent in each game.

The scoring system is as follows:

If the player wins a game, they score 2 points.
If the game is a tie, they score 1 point.
If they lose, they score 0 points.
For example, if the input string is "RSPRS":

If the player chooses all Rock, they would win games 2 and 5 (against Scissors), and tie games 1 and 4 (against Rock), scoring 22 + 21 = 6 points.
If they choose all Paper, they would win game 1 (against Rock), and tie games 2 and 5 (against Paper), scoring 21 + 21 = 4 points.
If they choose all Scissors, they would win games 3 (against Paper), and tie games 2 and 5 (against Scissors), scoring 21 + 21 = 4 points.
So, the maximum score they could achieve is 6 points, by choosing all Rock. Therefore, Solution("RSPRS") returns 6.

```go
func Solution(G string) int {
    countR, countP, countS := 0, 0, 0

    for _, gesture := range G {
        switch gesture {
        case 'R':
            countR++
        case 'P':
            countP++
        case 'S':
            countS++
        }
    }

    scoreR := countR*1 + countS*2 // Rock ties with Rock and wins Scissors
    scoreP := countP*1 + countR*2 // Paper ties with Paper and wins Rock
    scoreS := countS*1 + countP*2 // Scissors ties with Scissors and wins Paper

    return max(scoreR, scoreP, scoreS)
}

func max(a, b, c int) int {
    if a > b {
        if a > c {
            return a
        }
        return c
    }
    if b > c {
        return b
    }
    return c
}
```
