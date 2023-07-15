### Slot Machine Game 

This is a simple commandline slot mchine gamethat has been implemented n javascript. the game allows the user to deposit money, place their bets on multiple lines see if they've won and collect their innings. The game then continues if the user chooses to continue and stps when the user decides to stp or as run out of money. 

### Prerequisites
Before running the game make sure you have the following installed:
- Node.js (version 12 or higher)

### Installation
1. Open your terminal on visual studio code and navigate to the project directory
2. Next, run the following command on the terminal to install the dependencies:
    npm install

### Writing the Code
The code includes various functions that make different aspects of the game work. 
1. 'deposit()': This promts the user to enter a deposit amount and it then validates the input. It then returns the valid deposit amount as a number.
        const deposit = () => {
            while (true) {
                const depositAmount = promt("Enter a deposit amount: ");
                const numberDepositAmount = parseFloat(depositAmount);

                if (isNaN(numberDepositAmount) || numberDepositAmount <= 0) {
                console.log("Invaid deposit amount, try again.");
            } else {
                return numberDepositAmount;
            }
        }
    };

2. 'getNumberOfLines()': This prompts the user to enter the number of lines hat they want to bet on. It then validatesthe input. It return the valid number of lines as a number. 
        const getNumberOfLines = () => {
            while (true) {
                const lines = promt("Enter the number of lines to bet on (1-3): ");
                const numberOfLines = parseFloat(lines);

                        if (isNaN(numberOfLines) || numberOfLines <= 0 || numberOfLines > 3) {
                console.log("Invaid number of lines, try again.");
            } else {
                return numberOfLines;
            }
        }
    };

3. 'getBet(balance, lines)': This prompts the user to enter the bet amount per line and then validates the input based on the available balance and number of lines. It then eturn the valid bet amount as a number. 
        const getBet = (balance, lines) => {
            while (true) {
                const bet = promt("Enter the bet per line: ");
                const numberBet = parseFloat(bet);

                if (isNaN(numberBet) || numberBet <= 0 || numberBet > balance / lines) {
                    console.log("Invaid bet, try again.");
                } else {
                    return numberBet;
            }
        } 
    };

4. 'spin()': This simulates the spinning of the slot machine reels. It will randomly select symbols for each reel and return the resulting reel.

            const spin = () => {
        const symbols = [];
        for (const [symbol, count] of Object.entries(SYMBOLS_COUNT)) {
            for (let i = 0; i < count; i++) {
                symbols.push(symbol);
            }
        }
        const reels =[];
        for (let i = 0; i < COLS; i++) {
            reels.push([]);
            const reelSymbols = [...symbols];
            for (let j = 0; j < ROWS; j++) {
                const randomIndex = Math.floor(Math.random() * reelSymbols.length);
                const selectedSymbol = reelSymbols[randomIndex];
                reels[i].push(selectedSymbol);
                reelSymbols.splice(randomIndex, 1);
            }
        }
        return reels;
    };

5. 'transpose(reels)': This transposes the reels matrix to group symbols by row instead of by column. It then returns a new matrix representing the rows of symbols. 
        const transpose = (reels) => {
        const rows = [];

        for (let i = 0; i < ROWS; i++) {
            rows.push([]);
            for (let j =  0; j < COLS; j++) {
                rows[i].push(reels[j][i]);
            }
                
        }

        return rows;
    };

6. 'printRows(rows)': This prints the rows of symbols in a visually appealing format. 
        const printRows = (rows) => {
        for (const row of rows) {
            let rowString = "";
            for (const [i, symbol] of row.entries()) {
                rowString += symbol;
                if (i != row.length - 1) {
                    rowString += " | "
                }
            }
            console.log(rowString);
        }
    };

7. 'getWinnings(rows, bet, lines)': This calculates the user's winnings based on the symbols displayed on the rows, the bet amount, and the number of lines. It returns the total winnings as a number. 
        const getWinnings = (rows, bet, lines) => {
        let winnings = 0;

        for (let row= 0; row < lines; row++) {
            const symbols = rows[row];
            let allSame = true;

            for (const symbol of symbols) {
                if (symbol != symbols[0]) {
                    allSame = false;
                    break;
                }
            }

            if (allSame) {
                winnings += bet * SYMBOL_VALUES[symbols[0]];
            }
        }

        return winnings;
    };

8. 'game()': This implements he main game loop. This function manages the user's balance, calls other function to handle every other step of the game, and then determines when t contiue or end the game basd on the alance and the user input. 
        const game = () => {
        let balance = deposit();

            while (true) {
                console.log("You have a balance of $" + balance)
                const numberOfLines = getNumberOfLines();
                const bet = getBet(balance, numberOfLines);
                balance -= bet * numberOfLines; 
                const reels = spin();
                const rows = transpose(reels); 
                printRows(rows);
                const winnings = getWinnings(rows, bet, numberOfLines)
                balance += winnings;
                console.log("you have won, $" + winnings.toString());

                if (balance <= 0) {
                    console.log("You have run out of money!");
                    break;
                    }

                    const playAgain = prompt ("Would you like to play again (y/n)? ");

                    if (playAgain != "y") break;

                }
    };

### Codes Explained
The above code starts by definig the constanrs fr the number of rows, columns, symbo counts, and symbol  values used in the game. 

Then the code defines each of the functions mentioned above. thesefunctions handle input validation, random symbol selection, calculations and printing. 

After the funtion are defined,the'game()' function is called to start the game loop. The user is then prompted to enter a deposit amount, and then the game will continue unti te user runs outof money or deccides o stop playing. Each iteration of the game follows this sequencee: 
deposit -> number of lines -> bet amount -> spin -> print rows -> calcuate winnings -> update balance -> check if the user wants to play again. 

