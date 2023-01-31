---
layout: post
title: "Applying functions to create RedBlink"
date: 2022-09-16 21:00:00 -0500
categories: GoldenBrain
---

## Introduction

---

RedBlink is a memory game created by Tres Fellas that is in development. It is created in order to prevent memory loss of middle-aged people, who may have the potential to develop diseases such as Alzheimer's. This thoughtful game was created by Swift, which also includes basic functions that is important in any software applications.

### Game Mechanics

Before explaining the main functions of the game, it is important to understand the game mechanics. Initially, one red square cell will appear among the grey squared cells. At every round, a new red cell is added. It is the task of the player to find the most recent cell that turned red until all cells become red.

### Pause Function

```swift
private func blinkAndAddNewCell() {
    //How to blink
    //1. Red Cell -> Gray Cell
    //2. Select new random cell
    //3. Gray Cell -> Red Cell
    //Takes 1 sec
    showColoredCells = false //Red Cell -> Gray Cell
    selectNextCellIndex() //Select new random cell
    
    DispatchQueue.main.asyncAfter(deadline: .now() + 1) { //Takes 1 sec
      self.showColoredCells = true //Gray Cell -> Red Cell
    }
  }
```

The function `blinkAndAddNewCell` was created to progress each round by making all red cells from the previous round disappear, adding a new cell to be turned into red, and then making all red cells to reappear. The variable `showColoredCells` is like a switch for turning on and off the red cells.

```Swift
private func selectNextCellIndex() {
    redCellIndices.append(newCellCandidates.removeFirst())
  }
```

After turning off the red cells, `selectNextCellIndex` takes part in adding a new red cell. `redCellIndices` adds by removing the first element from the `newCellCandidates` array. This array stores the ascending numbers between 0 and the total number of cells -1, which is shuffled to create a random list of cells to be turned into red.

After using `DispatchQueue` to delay the game, the red cells are turned back on, which now includes one additional red cell that was just added.

### Check Condition Function

```swift
func checkUserSelection(userSelection: Int) {
    //If Correct -> Next Stage
    //If not correct -> bust: show green circle
    
    if userSelection == redCellIndices.last { //If Correct -> Next Stage
      //If there is another round -> go to the next round
      //If this is the last round, you win!
      //If there is another round -> go to the next round {
      if round == numberOfRows * numberOfRows - 1 {
        finishGame()
      } else {
        proceedToNextRound()
      }
    } else { //If wrong
      bustGame()
    }
  }
```

`checkUserSelection` checks every round whehter the player has selected the most recently added red cell. If they did, it will also check for the number of rounds to see if the player has completed the game. If all cells are turned red, the number of rounds would have reached the total number of cells -1, which leads to executing `finishGame` to finish the game. The game will continue with `proceedToNextRound`.

If the player was wrong, then the game will execute `bustGame`, which will lead to ending the game while showing the player what the correct answer was during that round.