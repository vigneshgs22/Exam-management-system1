import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;

class ExamSystem {

    JFrame frame;
    CardLayout layout = new CardLayout();
    JPanel mainPanel = new JPanel(layout);

    ArrayList<String> questions = new ArrayList<>();
    ArrayList<String> answers = new ArrayList<>();
    int score = 0, currentQuestion = 0;

    ExamSystem() {
        frame = new JFrame("Exam Management System");
        frame.setSize(600, 400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        mainPanel.add(loginPanel(), "Login");
        mainPanel.add(adminPanel(), "Admin");
        mainPanel.add(studentPanel(), "Student");
        mainPanel.add(examPanel(), "Exam");

        frame.add(mainPanel);
        frame.setVisible(true);
        layout.show(mainPanel, "Login");
    }

    JPanel loginPanel() {
        JPanel panel = new JPanel(null);
        JLabel userLabel = new JLabel("Username:");
        JTextField username = new JTextField();
        JLabel passLabel = new JLabel("Password:");
        JPasswordField password = new JPasswordField();
        JButton loginButton = new JButton("Login");

        userLabel.setBounds(180, 100, 80, 30);
        username.setBounds(270, 100, 120, 30);
        passLabel.setBounds(180, 140, 80, 30);
        password.setBounds(270, 140, 120, 30);
        loginButton.setBounds(230, 200, 100, 30);

        loginButton.addActionListener(e -> {
            String user = username.getText();
            String pass = new String(password.getPassword());
            if ("Vignesh".equals(user) && "vignesh123".equals(pass)) layout.show(mainPanel, "Admin");
            else if ("vigneshkrct".equals(user) && "vigneshkrct".equals(pass)) layout.show(mainPanel, "Student");
            else JOptionPane.showMessageDialog(frame, "Invalid credentials");
        });

        panel.add(userLabel); panel.add(username); panel.add(passLabel);
        panel.add(password); panel.add(loginButton);
        return panel;
    }

    JPanel adminPanel() {
        JPanel panel = new JPanel(null);
        JLabel title = new JLabel("Admin Dashboard");
        JTextArea questionField = new JTextArea();
        JTextField answerField = new JTextField();
        JButton addQuestionButton = new JButton("Add Question");
        JButton logoutButton = new JButton("Logout");

        title.setBounds(220, 20, 160, 30);
        questionField.setBounds(50, 70, 500, 100);
        answerField.setBounds(50, 190, 500, 30);
        addQuestionButton.setBounds(220, 240, 160, 30);
        logoutButton.setBounds(10, 10, 100, 30);

        addQuestionButton.addActionListener(e -> {
            String question = questionField.getText().trim();
            String answer = answerField.getText().trim();
            if (!question.isEmpty() && !answer.isEmpty()) {
                questions.add(question);
                answers.add(answer);
                questionField.setText("");
                answerField.setText("");
                JOptionPane.showMessageDialog(frame, "Question Added!");
            } else {
                JOptionPane.showMessageDialog(frame, "Both fields are required!");
            }
        });

        logoutButton.addActionListener(e -> layout.show(mainPanel, "Login"));

        panel.add(title); panel.add(questionField); panel.add(answerField);
        panel.add(addQuestionButton); panel.add(logoutButton);
        return panel;
    }

    JPanel studentPanel() {
        JPanel panel = new JPanel(null);
        JLabel title = new JLabel("Student Dashboard");
        JButton startExamButton = new JButton("Start Exam");
        JButton logoutButton = new JButton("Logout");

        title.setBounds(220, 20, 160, 30);
        startExamButton.setBounds(220, 100, 160, 30);
        logoutButton.setBounds(10, 10, 100, 30);

        startExamButton.addActionListener(e -> {
            if (questions.isEmpty()) {
                JOptionPane.showMessageDialog(frame, "No questions available.");
            } else {
                score = 0;
                currentQuestion = 0;
                layout.show(mainPanel, "Exam");
            }
        });

        logoutButton.addActionListener(e -> layout.show(mainPanel, "Login"));

        panel.add(title); panel.add(startExamButton); panel.add(logoutButton);
        return panel;
    }

    JPanel examPanel() {
        JPanel panel = new JPanel(null);
        JLabel questionLabel = new JLabel();
        JTextField answerField = new JTextField();
        JButton nextButton = new JButton("Next");
        JButton logoutButton = new JButton("Logout");

        questionLabel.setBounds(50, 50, 500, 50);
        answerField.setBounds(50, 120, 500, 30);
        nextButton.setBounds(220, 170, 160, 30);
        logoutButton.setBounds(10, 10, 100, 30);

        nextButton.addActionListener(e -> {
            if (answers.get(currentQuestion).equalsIgnoreCase(answerField.getText().trim())) {
                score++;
            }
            currentQuestion++;
            if (currentQuestion < questions.size()) {
                questionLabel.setText(questions.get(currentQuestion));
                answerField.setText("");
            } else {
                JOptionPane.showMessageDialog(frame, "Exam Over! Your Score: " + score);
                layout.show(mainPanel, "Student");
            }
        });

        logoutButton.addActionListener(e -> layout.show(mainPanel, "Login"));

        panel.add(questionLabel); panel.add(answerField);
        panel.add(nextButton); panel.add(logoutButton);
        panel.addComponentListener(new ComponentAdapter() {
            public void componentShown(ComponentEvent e) {
                if (!questions.isEmpty()) {
                    questionLabel.setText(questions.get(currentQuestion));
                }
            }
        });

        return panel;
    }

    public static void main(String[] args) {
        new ExamSystem();
    }
}
