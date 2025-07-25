---
title: "AI Chess"
category: [AI, Unity]
layout: page
image: /assets/img/AI_Chess.png
order: 6
---


This class project was developed by a team of three using __Unity__ with the goal of researching and comparing different algorithms that can play chess. As part of our exploration, we built a demo where users can play against various AI opponents we developed.

Our focus was on finding a balance between the speed of decision-making and the quality of the moves chosen by the AI. We implemented several algorithms and compared them based on both their performance (number of wins) and their computational efficiency (decision time). This hands-on approach allowed us to better understand the trade-offs between fast decision-making and strategic depth in AI gameplay.

## Demo
{% 
    include embed/youtube.html id='rpwxKHAHHrE' 
%}

## Minimax Algorithm
> It evaluates possible moves by exploring a game tree, where players alternate turns as either the maximizing or minimizing player. The algorithm simulates all possible moves until it reaches terminal nodes (end states), then works its way back up the tree to choose the optimal move.

```c#
if (depth == 0)
{
    return EvaluateBoard(board);
}

if (isMaximizingPlayer)
{
    int maxEval = int.MinValue;
    var allPieces = board.Pieces.Values.Where(p => p.Piece.IsWhite == IsWhite).ToList();

    foreach (var piece in allPieces)
    {
        var possibleMoves = piece.GetPossibleMoves(board).ToList();
        foreach (var move in possibleMoves)
        {
            var originalPosition = piece.Position;
            var targetPiece = board.Pieces.ContainsKey(move) ? board.Pieces[move] : null;

            // Make the move
            piece.Move(move);
            board.Pieces[move] = piece;
            board.Pieces.Remove(originalPosition);

            int eval = Minimax(board, depth - 1, false);

            // Undo the move
            piece.Move(originalPosition);
            board.Pieces[originalPosition] = piece;
            if (targetPiece != null)
            {
                board.Pieces[move] = targetPiece;
            }
            else
            {
                board.Pieces.Remove(move);
            }

            maxEval = Math.Max(maxEval, eval);
        }
    }

    return maxEval;
}
else
{
    int minEval = int.MaxValue;
    var allPieces = board.Pieces.Values.Where(p => p.Piece.IsWhite != IsWhite).ToList();

    foreach (var piece in allPieces)
    {
        var possibleMoves = piece.GetPossibleMoves(board).ToList();
        foreach (var move in possibleMoves)
        {
            var originalPosition = piece.Position;
            var targetPiece = board.Pieces.ContainsKey(move) ? board.Pieces[move] : null;

            // Make the move
            piece.Move(move);
            board.Pieces[move] = piece;
            board.Pieces.Remove(originalPosition);

            int eval = Minimax(board, depth - 1, true);

            // Undo the move
            piece.Move(originalPosition);
            board.Pieces[originalPosition] = piece;
            if (targetPiece != null)
            {
                board.Pieces[move] = targetPiece;
            }
            else
            {
                board.Pieces.Remove(move);
            }

            minEval = Math.Min(minEval, eval);
        }
    }

    return minEval;
}
```
## Alpha-Beta Pruning
> This is a optimization based of the minimax algorithm. It speeds up the decision-making process by eliminating branches in the game tree that don’t need to be explored because they cannot influence the final decision. 
> * Alpha refers to the best score the maximizing player can get.
> * Beta refers to the best score the minimizing player can get.
>
> If the move leads to worse output than the previous move it skips the current branch.

```c#
if (isMaximizingPlayer)
{
    if (boardValue > bestValue)
    {
        bestValue = boardValue;
        bestMove = (piece, move);
    }
    alpha = Math.Max(alpha, boardValue);
}
else
{
    if (boardValue < bestValue)
    {
        bestValue = boardValue;
        bestMove = (piece, move);
    }
    beta = Math.Min(beta, boardValue);
}

if (beta <= alpha)
{
    // Undo the move
    piece.Move(originalPosition);
    board.Pieces[originalPosition] = piece;
    if (targetPiece != null)
    {
        board.Pieces[move] = targetPiece;
    }
    else
    {
        board.Pieces.Remove(move);
    }
    return bestMove;
}
```