import javax.swing.*;

import java.awt.*;

import java.awt.event.*;

import javax.sound.sampled.*;


import java.io.File;

import java.io.IOException;


 class KeyListenerWithSoundExample extends JFrame implements KeyListener {

    private JLabel label;
    private JComboBox<String> soundComboBox;
    private String[] soundOptions = {"Chik Chik", "Bip Bip", "Ding Dong"};

    public KeyListenerWithSoundExample() {
        // Set up the frame
        setTitle("Key Listener with Sound Example");
        setSize(400, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);  // Center the window
        setLayout(new BorderLayout());

        // Create a label to display key events
        label = new JLabel("Press any key...", SwingConstants.CENTER);
        label.setFont(label.getFont().deriveFont(16f));  // Set font size
        add(label, BorderLayout.CENTER);

        // Create a JComboBox to select sound
        soundComboBox = new JComboBox<>(soundOptions);
        soundComboBox.setSelectedIndex(0);  // Default selection
        add(soundComboBox, BorderLayout.NORTH);

        // Add the key listener to the frame
        addKeyListener(this);

        // Make sure the frame is focusable so it can capture key events
        setFocusable(true);
    }

    // Method to play sound based on selected option
    private void playSound(String soundFile) {
        try {
            // Create a file object for the sound file
            File sound = new File(soundFile);

            if (!sound.exists()) {
                System.err.println("Error: Sound file not found: " + soundFile);
                return;
            }

            // Get an audio stream for the file
            AudioInputStream audioIn = AudioSystem.getAudioInputStream(sound);
            Clip clip = AudioSystem.getClip();
            clip.open(audioIn);
            clip.start();  // Start playing the sound
            System.out.println("Playing sound: " + soundFile);  // Debugging
        } catch (UnsupportedAudioFileException e) {
            System.err.println("Error: Unsupported audio file format.");
        } catch (IOException e) {
            System.err.println("Error: Could not read the sound file.");
        } catch (LineUnavailableException e) {
            System.err.println("Error: Audio line unavailable.");
        }
    }

    // KeyListener methods with sound effect
    @Override
    public void keyTyped(KeyEvent e) {
        label.setText("Key typed: " + e.getKeyChar());
        playSelectedSound();  // Play the selected sound on key typed
    }

    @Override
    public void keyPressed(KeyEvent e) {
        label.setText("Key pressed: " + e.getKeyChar());
        playSelectedSound();  // Play the selected sound on key pressed
    }

    @Override
    public void keyReleased(KeyEvent e) {
        label.setText("Key released: " + e.getKeyChar());
        playSelectedSound();  // Play the selected sound on key released
    }

    // Method to play the selected sound from JComboBox
    private void playSelectedSound() {
        String selectedSound = (String) soundComboBox.getSelectedItem();
        switch (selectedSound) {
            case "Chik Chik":
                playSound("D:\\good.wav");
                break;
            case "Bip Bip":
                playSound("bip_bip.wav");
                break;
            case "Ding Dong":
                playSound("ding_dong.wav");
                break;
            default:
                System.out.println("Unknown sound selected");
                break;
        }
    }

    // Main method to run the application
    public static void main(String[] args) {
        // Create and display the frame
        SwingUtilities.invokeLater(() -> {
            KeyListenerWithSoundExample frame = new KeyListenerWithSoundExample();
            frame.setVisible(true);
        });
    }
}
