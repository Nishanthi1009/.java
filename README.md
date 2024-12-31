package wins;
import javax.swing.*;
import javax.swing.border.LineBorder;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Objects;
import java.util.Random;

public class AWT_TicTacToe implements ActionListener {
    JFrame frame = new JFrame();
    JPanel titlePanel = new JPanel();
    JPanel buttonPanel = new JPanel();
    JLabel label = new JLabel();
    JButton[] buttons = new JButton[9];
    JButton playAgainButton = new JButton("Play Again");
    boolean playerTurn;  // True for "X", False for "O"
    boolean gameOver;
    String playerChoice = "X";  // Default choice for player
    String computerChoice = "O";  // Default choice for computer

    // Method to generate a random color
    private Color generateRandomColor() {
        Random rand = new Random();
        return new Color(rand.nextInt(256), rand.nextInt(256), rand.nextInt(256));  // RGB values between 0 and 255
    }

    AWT_TicTacToe() {
        // Set up the frame
        ImageIcon icon = new ImageIcon("TicTacToe_Logo.png");
        frame.setIconImage(icon.getImage());
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(800, 800);
        frame.setLayout(new BorderLayout());

        // Title panel setup with background color and font
        label.setText("Tic Tac Toe: Choose your symbol");
        label.setHorizontalAlignment(JLabel.CENTER);
        label.setFont(new Font("Arial", Font.BOLD, 60));
        label.setBackground(new Color(34, 34, 34));  // Dark background
        label.setForeground(new Color(255, 255, 255));  // White text color
        label.setOpaque(true);

        titlePanel.setLayout(new BorderLayout());
        titlePanel.setBounds(0, 0, 800, 100);
        titlePanel.add(label);

        // Button panel (game grid) setup with background color
        buttonPanel.setLayout(new GridLayout(3, 3));
        buttonPanel.setBackground(new Color(50, 50, 50));  // Dark gray background

        // Create the buttons and customize
        for (int i = 0; i < 9; i++) {
            buttons[i] = new JButton();
            buttonPanel.add(buttons[i]);
            buttons[i].setBorder(new LineBorder(Color.BLACK));
            buttons[i].setFont(new Font("Calibri", Font.BOLD, 120));
            buttons[i].setFocusable(false);
            buttons[i].setBackground(generateRandomColor());  // Set a random color for each button initially
            buttons[i].addActionListener(this);
        }

        // Play Again Button Setup
        playAgainButton.setFont(new Font("Arial", Font.BOLD, 40));
        playAgainButton.setFocusable(false);
        playAgainButton.setBackground(new Color(0, 255, 0));  // Green background
        playAgainButton.addActionListener(e -> resetGame());

        JPanel playAgainPanel = new JPanel();
        playAgainPanel.add(playAgainButton);
        playAgainPanel.setBackground(new Color(50, 50, 50));  // Dark gray background for play again panel

        // Adding components to the frame
        frame.add(titlePanel, BorderLayout.NORTH);
        frame.add(buttonPanel);
        frame.add(playAgainPanel, BorderLayout.SOUTH);

        frame.setVisible(true);  // Make the frame visible after adding all components
        resetGame(); // Initialize the game
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (gameOver) return;

        for (int i = 0; i < 9; i++) {
            if (e.getSource() == buttons[i]) {
                if (buttons[i].getText().isEmpty()) {
                    if (playerTurn) {
                        buttons[i].setForeground(new Color(255, 0, 0)); // Red color for player "X"
                        buttons[i].setText(playerChoice);
                        label.setText("Tic Tac Toe: " + computerChoice + " turn");
                    }
                    playerTurn = !playerTurn;
                    changeButtonColors();  // Update button colors based on turn
                    check();

                    // If it's the computer's turn, make the move automatically
                    if (!playerTurn && !gameOver) {
                        makeAutoMove();
                    }
                }
            }
        }
    }

    // Automatic move for the computer (Player O)
    private void makeAutoMove() {
        Random rand = new Random();
        boolean moveMade = false;

        while (!moveMade) {
            int index = rand.nextInt(9); // Randomly choose an index (0 to 8)

            if (buttons[index].getText().isEmpty()) {
                buttons[index].setForeground(new Color(0, 0, 255)); // Blue color for computer "O"
                buttons[index].setText(computerChoice);
                label.setText("Tic Tac Toe: " + playerChoice + " turn");
                playerTurn = !playerTurn;
                moveMade = true;
                changeButtonColors();  // Update button colors based on turn
                check();
            }
        }
    }

    // Update button colors based on the current player's turn
    private void changeButtonColors() {
        for (int i = 0; i < 9; i++) {
            buttons[i].setBackground(generateRandomColor());  // Assign a random color to each button
        }
    }

    // Check for winner or draw
    public void check() {
        if (Objects.equals(buttons[0].getText(), buttons[1].getText()) && Objects.equals(buttons[1].getText(), buttons[2].getText())) {
            if (Objects.equals(buttons[0].getText(), "X") || Objects.equals(buttons[0].getText(), "O"))
                winner(0, 1, 2);
        }
        if (Objects.equals(buttons[3].getText(), buttons[4].getText()) && Objects.equals(buttons[4].getText(), buttons[5].getText())) {
            if (Objects.equals(buttons[3].getText(), "X") || Objects.equals(buttons[3].getText(), "O"))
                winner(3, 4, 5);
        }
        if (Objects.equals(buttons[6].getText(), buttons[7].getText()) && Objects.equals(buttons[7].getText(), buttons[8].getText())) {
            if (Objects.equals(buttons[6].getText(), "X") || Objects.equals(buttons[6].getText(), "O"))
                winner(6, 7, 8);
        }
        if (Objects.equals(buttons[0].getText(), buttons[3].getText()) && Objects.equals(buttons[3].getText(), buttons[6].getText())) {
            if (Objects.equals(buttons[0].getText(), "X") || Objects.equals(buttons[0].getText(), "O"))
                winner(0, 3, 6);
        }
        if (Objects.equals(buttons[1].getText(), buttons[4].getText()) && Objects.equals(buttons[4].getText(), buttons[7].getText())) {
            if (Objects.equals(buttons[1].getText(), "X") || Objects.equals(buttons[1].getText(), "O"))
                winner(1, 4, 7);
        }
        if (Objects.equals(buttons[2].getText(), buttons[5].getText()) && Objects.equals(buttons[5].getText(), buttons[8].getText())) {
            if (Objects.equals(buttons[2].getText(), "X") || Objects.equals(buttons[2].getText(), "O"))
                winner(2, 5, 8);
        }
        if (Objects.equals(buttons[0].getText(), buttons[4].getText()) && Objects.equals(buttons[4].getText(), buttons[8].getText())) {
            if (Objects.equals(buttons[0].getText(), "X") || Objects.equals(buttons[0].getText(), "O"))
                winner(0, 4, 8);
        }
        if (Objects.equals(buttons[2].getText(), buttons[4].getText()) && Objects.equals(buttons[4].getText(), buttons[6].getText())) {
            if (Objects.equals(buttons[2].getText(), "X") || Objects.equals(buttons[2].getText(), "O"))
                winner(2, 4, 6);
        }

        // Check if the game is a draw
        boolean allButtonsFilled = true;
        for (int i = 0; i < 9; i++) {
            if (Objects.equals(buttons[i].getText(), "")) {
                allButtonsFilled = false;
                break;
            }
        }
        if (allButtonsFilled && !gameOver) {
            label.setText("It's a draw!");
            gameOver = true;
            disableButtons();
        }
    }

    // Declare winner
    public void winner(int p, int q, int r) {
        buttons[p].setBackground(new Color(0, 255, 0));  // Green color for winning combination
        buttons[q].setBackground(new Color(0, 255, 0));
        buttons[r].setBackground(new Color(0, 255, 0));
        label.setText((buttons[p].getText() + " Wins!"));
        gameOver = true;
        disableButtons();
    }

    // Disable all buttons
    private void disableButtons() {
        for (int i = 0; i < 9; i++) {
            buttons[i].setEnabled(false);
        }
    }

    // Reset the game
    private void resetGame() {
        // Ask the user to choose "X" or "O"
        String[] choices = {"X", "O"};
        playerChoice = (String) JOptionPane.showInputDialog(frame, "Choose your symbol: ",
                "Tic Tac Toe", JOptionPane.QUESTION_MESSAGE, null, choices, "X");
        if (playerChoice == null) {
            playerChoice = "X"; // Default to "X" if no choice made
        }
        computerChoice = playerChoice.equals("X") ? "O" : "X";  // Set the computer's choice

        for (int i = 0; i < 9; i++) {
            buttons[i].setText("");
            buttons[i].setEnabled(true);
            buttons[i].setBackground(generateRandomColor()); // Assign random colors to buttons when resetting
        }
        label.setText("Tic Tac Toe: " + playerChoice + " turn");
        gameOver = false;
        playerTurn = playerChoice.equals("X");  // Player "X" starts first
    }

    // Main method to start the game
    public static void main(String[] args) {
        // Create an instance of the TicTacToe game
        SwingUtilities.invokeLater(() -> new AWT_TicTacToe());
    }
}

