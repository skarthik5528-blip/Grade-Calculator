import java.awt.*;
import java.awt.event.*;
import javax.swing.*;

public class Calculator1 extends JFrame implements ActionListener {
    private JTextField txtId, txtSection, txtName;
    private JTextField tx1, tx2, tx3, tx4, tx5, tx6;
    private JTextField txtTotal, txtPercentage, txtGrade;
    private JButton b1, b2, b3;
    private JComboBox<String> cb1; // ComboBox for section

    public Calculator1() {
        setTitle("Ballari Institute Of Tecnology & Management");
        setSize(700, 700);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        setLayout(null);

        JLabel l1 = new JLabel("Ballari Institute Of Tecnology & Management", SwingConstants.CENTER);
        l1.setFont(new Font("Tahoma", Font.BOLD, 28));
        l1.setBounds(40, 10, 600, 40);
        add(l1);

        addLabel("Student ID:", 40, 70, 100, 25);
        txtId = addTextField(140, 70, 80, 25);

        addLabel("Section", 260, 70, 80, 25);
        txtSection = addTextField(320, 70, 60, 25);

        addLabel("Student Name:", 40, 110, 120, 25);
        txtName = addTextField(160, 110, 240, 25);

        // Marks
        addLabel("DSDCO", 40, 160, 80, 25);
        tx1 = addTextField(120, 160, 80, 25);

        addLabel("O.S", 40, 200, 80, 25);
        tx3 = addTextField(120, 200, 80, 25);

        addLabel("Maths", 40, 240, 80, 25);
        tx2 = addTextField(120, 240, 80, 25);

        addLabel("JAVA", 40, 280, 80, 25);
        tx4 = addTextField(120, 280, 80, 25);

        addLabel("UNIX", 40, 320, 80, 25);
        tx5 = addTextField(120, 320, 80, 25);

        addLabel("DSA", 40, 360, 80, 25);
        tx6 = addTextField(120, 360, 80, 25);

        addLabel("Total Marks", 40, 400, 100, 25);
        txtTotal = addTextField(120, 400, 120, 25);
        txtTotal.setEditable(false);

        addLabel("Percentage", 260, 400, 100, 25);
        txtPercentage = addTextField(340, 400, 120, 25);
        txtPercentage.setEditable(false);

        addLabel("Grade", 40, 460, 80, 25);
        txtGrade = addTextField(120, 460, 60, 25);
        txtGrade.setEditable(false);

        // Buttons
        b1 = new JButton("Calculate");
        b1.setBounds(100, 500, 120, 35);
        b1.addActionListener(this);
        add(b1);

        b2 = new JButton("Clear");
        b2.setBounds(260, 500, 120, 35);
        b2.addActionListener(this);
        add(b2);

        b3 = new JButton("Exit");
        b3.setBounds(420, 500, 120, 35);
        b3.addActionListener(this);
        add(b3);

        getContentPane().setBackground(new Color(215, 205, 235));
        setVisible(true);
    }

    private void addLabel(String text, int x, int y, int w, int h) {
        JLabel lbl = new JLabel(text);
        lbl.setBounds(x, y, w, h);
        add(lbl);
    }

    private JTextField addTextField(int x, int y, int w, int h) {
        JTextField tf = new JTextField();
        tf.setBounds(x, y, w, h);
        add(tf);
        return tf;
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        Object src = e.getSource();
        if (src == b1) {
            calculateGrades();
        } else if (src == b2) {
            clearFields();
        } else if (src == b3) {
            int option = JOptionPane.showConfirmDialog(this, "Do you really want to exit?", "Exit",
                    JOptionPane.YES_NO_OPTION);
            if (option == JOptionPane.YES_OPTION) {
                System.exit(0);
            }
        }
    }

    private void calculateGrades() {
        try {
            String name = txtName.getText().trim();
            String section = txtSection.getText().trim();

            double p = parseMark(tx1.getText()); // DSDCO
            double m = parseMark(tx2.getText()); // Maths
            double c = parseMark(tx3.getText()); // O.S
            double e = parseMark(tx4.getText()); // JAVA
            double a = parseMark(tx5.getText()); // UNIX
            double b = parseMark(tx6.getText()); // DSA

            // total and percentage
            double sum = p + m + c + e + a + b;
            double percentage = sum / 6.0;

            // grade logic
            String grade;
            if (percentage >= 90)
                grade = "A";
            else if (percentage >= 80)
                grade = "B";
            else if (percentage >= 70)
                grade = "C";
            else if (percentage >= 60)
                grade = "D";
            else
                grade = "F";

            // display results
            txtTotal.setText(String.format("%.1f", sum));
            txtPercentage.setText(String.format("%.2f", percentage));
            txtGrade.setText(grade);

            // popup message
            String message = String.format("Hello: %s of class: %s%nYour Grade is: %s",
                    name.isEmpty() ? "Student" : name,
                    section.isEmpty() ? "-" : section,
                    grade);
            JOptionPane.showMessageDialog(this, message, "Message", JOptionPane.INFORMATION_MESSAGE);

        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(this, "Please enter valid numeric marks (0-100).", "Input Error",
                    JOptionPane.ERROR_MESSAGE);
        }
    }

    private double parseMark(String s) {
        s = s.trim();
        if (s.isEmpty())
            return 0.0;
        double v = Double.parseDouble(s);
        if (v < 0 || v > 100)
            throw new NumberFormatException("mark out of range");
        return v;
    }

    private void clearFields() {
        txtId.setText("");
        txtSection.setText("");
        txtName.setText("");
        tx1.setText("");
        tx2.setText("");
        tx3.setText("");
        tx4.setText("");
        tx5.setText("");
        tx6.setText("");
        txtTotal.setText("");
        txtPercentage.setText("");
        txtGrade.setText("");
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(Calculator1::new);
    }
}
