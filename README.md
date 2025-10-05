Airport Management System

The Airport Management System is a Java-based desktop application developed using Java Swing and MySQL to manage airline data efficiently. It provides a graphical interface that allows users to perform essential database operations such as inserting, updating, deleting, and viewing airline records. This project highlights the integration of Java GUI components with MySQL using JDBC (Java Database Connectivity) for smooth data management.

The application begins with a secure login window, ensuring that only authorized users can access the main system. After successful login, users are directed to the main dashboard, where airline information is displayed in a structured table format. The interface is built using Java Swing components like JFrame, JTable, JButton, and JTextField, designed with attractive colors and a user-friendly layout.

The backend uses MySQL to store and retrieve airline details such as airline ID, name, fleet size, headquarters, and revenue. Using JDBC, the application connects to the MySQL database and executes SQL queries for various operations. Each action, such as adding or deleting an airline, provides clear success or error messages to the user, ensuring smooth interaction and feedback.

Key features of this project include a login authentication system, the ability to perform full CRUD (Create, Read, Update, Delete) operations, real-time table refresh to display updated data, and a clear field option for easy data entry. The interface also includes color-coded buttons and neatly aligned table cells to improve readability and user experience.

To set up the project, users must create a MySQL database named “airlines” and define a table with columns for ID, Name, Fleet Size, Headquarters, and Revenue. The database credentials in the code can be customized to match the user’s MySQL setup. Additionally, the MySQL Connector/JAR file should be added to the project for database connectivity.

This project is an excellent learning example for students and beginners who want to understand how Java Swing applications can be integrated with a database system. It demonstrates practical implementation of event handling, CRUD operations, and database interaction, making it a complete mini-project for learning Java and MySQL together.

Author: Yuvan Shankar
Technologies Used: Java Swing, JDBC, MySQL
Category: Desktop Application / Database Management System


# Airport-management-system



import java.awt.*;
import java.sql.*; // Import event classes
import javax.swing.*;
import javax.swing.table.DefaultTableCellRenderer;
import javax.swing.table.DefaultTableModel;

// Main application class to start the login process
public class AirlinesApp {
    public static void main(String[] args) {
        // Run the login frame on the Event Dispatch Thread
        SwingUtilities.invokeLater(() -> {
            LoginFrame loginFrame = new LoginFrame();
            loginFrame.setVisible(true);
        });
    }
}

// Login Frame Class
class LoginFrame extends JFrame {
    private JTextField usernameField;
    private JPasswordField passwordField;
    private JButton loginButton;
    private JLabel errorLabel;

    public LoginFrame() {
        setTitle("Airlines Database Login");
        setSize(400, 250); // Adjusted size for login
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null); // Center the window
        setLayout(new BorderLayout(10, 10)); // Use BorderLayout

        // Panel for components
        JPanel panel = new JPanel(new GridBagLayout());
        panel.setBorder(BorderFactory.createEmptyBorder(20, 20, 20, 20));
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 5, 5, 5); // Padding
        gbc.fill = GridBagConstraints.HORIZONTAL;

        // Username Label and Field
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.anchor = GridBagConstraints.EAST;
        panel.add(new JLabel("Username:"), gbc);

        gbc.gridx = 1;
        gbc.gridy = 0;
        gbc.anchor = GridBagConstraints.WEST;
        usernameField = new JTextField(15); // Field size
        panel.add(usernameField, gbc);

        // Password Label and Field
        gbc.gridx = 0;
        gbc.gridy = 1;
        gbc.anchor = GridBagConstraints.EAST;
        panel.add(new JLabel("Password:"), gbc);

        gbc.gridx = 1;
        gbc.gridy = 1;
        gbc.anchor = GridBagConstraints.WEST;
        passwordField = new JPasswordField(15);
        panel.add(passwordField, gbc);

        // Error Label
        gbc.gridx = 0;
        gbc.gridy = 2;
        gbc.gridwidth = 2; // Span across two columns
        gbc.anchor = GridBagConstraints.CENTER;
        errorLabel = new JLabel(" "); // Start with empty space
        errorLabel.setForeground(Color.RED);
        errorLabel.setHorizontalAlignment(SwingConstants.CENTER);
        panel.add(errorLabel, gbc);

        // Login Button
        gbc.gridx = 0;
        gbc.gridy = 3;
        gbc.gridwidth = 2; // Span across two columns
        gbc.anchor = GridBagConstraints.CENTER;
        loginButton = new JButton("Login");
        loginButton.setFont(new Font("Segoe UI", Font.BOLD, 13));
        loginButton.setPreferredSize(new Dimension(100, 30)); // Button size
        panel.add(loginButton, gbc);

        // Add panel to frame
        add(panel, BorderLayout.CENTER);

        // Login Button Action Listener
        loginButton.addActionListener(e -> performLogin());

        // Allow login on Enter key press in password field
        passwordField.addActionListener(e -> performLogin());
    }

    private void performLogin() {
        String username = usernameField.getText();
        String password = new String(passwordField.getPassword()); // Get password safely

        // --- Basic Authentication ---
        // !! WARNING: Hardcoded credentials - NOT SECURE for production !!
        // Replace with database lookup or a more secure method in a real application
        if ("admin".equals(username) && "password123".equals(password)) {
            // Login successful
            dispose(); // Close the login window

            // Open the main Airlines Database window
            SwingUtilities.invokeLater(() -> {
                AirlinesDatabase dbFrame = new AirlinesDatabase();
                dbFrame.setVisible(true);
            });

        } else {
            // Login failed
            errorLabel.setText("Invalid username or password.");
            passwordField.setText(""); // Clear password field
            usernameField.requestFocus(); // Focus back on username
        }
    }
}


// Airlines Database Management Frame Class (Your original code, slightly modified)
class AirlinesDatabase extends JFrame {
    private JTable table;
    private DefaultTableModel model;
    private JTextField idField, nameField, fleetField, hqField, revenueField;

    // Database credentials - MODIFY AS NEEDED
    private final String DB_URL = "jdbc:mysql://localhost:3306/airlines";
    private final String DB_USER = "root";
    private final String DB_PASSWORD = "yuvan"; // Change this to your password

    public AirlinesDatabase() {
        setTitle("Airlines Management System");
        setSize(950, 550);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // Exit app when this window closes
        setLocationRelativeTo(null);
        setLayout(new BorderLayout());

        // --- Table Setup ---
        model = new DefaultTableModel(new String[]{"ID", "Name", "Fleet Size", "Headquarters", "Revenue"}, 0) {
            // Make table cells non-editable by default
             @Override
             public boolean isCellEditable(int row, int column) {
                return false;
             }
        };
        table = new JTable(model);
        table.setFillsViewportHeight(true);
        table.setRowHeight(28);
        table.setFont(new Font("Segoe UI", Font.PLAIN, 14));
        table.getTableHeader().setFont(new Font("Segoe UI", Font.BOLD, 14));
        table.setGridColor(Color.LIGHT_GRAY);
        table.setSelectionMode(ListSelectionModel.SINGLE_SELECTION); // Allow selecting only one row

        // Center align table content
        DefaultTableCellRenderer centerRenderer = new DefaultTableCellRenderer();
        centerRenderer.setHorizontalAlignment(SwingConstants.CENTER);
        for (int i = 0; i < model.getColumnCount(); i++) {
            table.getColumnModel().getColumn(i).setCellRenderer(centerRenderer);
        }

        // Add listener to populate form fields when a row is selected
        table.getSelectionModel().addListSelectionListener(event -> {
            if (!event.getValueIsAdjusting() && table.getSelectedRow() != -1) {
                populateFieldsFromSelectedRow();
            }
        });


        add(new JScrollPane(table), BorderLayout.CENTER);

        // --- Form Panel ---
        JPanel formPanel = new JPanel(new GridBagLayout());
        formPanel.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createTitledBorder("Airline Details"),
                BorderFactory.createEmptyBorder(5, 10, 10, 10) // Padding inside the border
        ));
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(4, 4, 4, 4); // Padding between components
        gbc.anchor = GridBagConstraints.WEST; // Align labels left

        // Labels
        gbc.gridx = 0; gbc.gridy = 0; formPanel.add(new JLabel("ID:"), gbc);
        gbc.gridx = 0; gbc.gridy = 1; formPanel.add(new JLabel("Name:"), gbc);
        gbc.gridx = 2; gbc.gridy = 0; formPanel.add(new JLabel("Fleet Size:"), gbc); // Column 3
        gbc.gridx = 2; gbc.gridy = 1; formPanel.add(new JLabel("Headquarters:"), gbc);
        gbc.gridx = 4; gbc.gridy = 0; formPanel.add(new JLabel("Revenue:"), gbc); // Column 5

        // Text Fields
        gbc.fill = GridBagConstraints.HORIZONTAL;
        gbc.weightx = 1.0; // Allow fields to expand horizontally

        gbc.gridx = 1; gbc.gridy = 0; idField = new JTextField(8); formPanel.add(idField, gbc); // Limited width
        gbc.gridx = 1; gbc.gridy = 1; nameField = new JTextField(20); formPanel.add(nameField, gbc);
        gbc.gridx = 3; gbc.gridy = 0; fleetField = new JTextField(8); formPanel.add(fleetField, gbc);
        gbc.gridx = 3; gbc.gridy = 1; hqField = new JTextField(20); formPanel.add(hqField, gbc);
        gbc.gridx = 5; gbc.gridy = 0; revenueField = new JTextField(10); formPanel.add(revenueField, gbc);

        // Make ID field read-only after initial load, enable for insert? No, let update use it.
        // Consider making ID non-editable visually too:
        // idField.setEditable(false);
        // idField.setBackground(Color.LIGHT_GRAY);


        gbc.gridx = 4; gbc.gridy = 1; // Empty cell for spacing
        gbc.weightx = 0.0; // Don't let the empty cell expand
        formPanel.add(Box.createHorizontalStrut(10), gbc); // Add some space


        add(formPanel, BorderLayout.NORTH);

        // --- Button Panel ---
        JPanel buttonPanel = new JPanel(new FlowLayout(FlowLayout.CENTER, 15, 10)); // Centered buttons with gaps
        JButton insertBtn = new JButton("Insert");
        JButton updateBtn = new JButton("Update");
        JButton deleteBtn = new JButton("Delete");
        JButton clearBtn = new JButton("Clear Fields"); // Added Clear button
        JButton refreshBtn = new JButton("Refresh");

        // Styling buttons
        Dimension btnSize = new Dimension(110, 30);
        Color insertColor = new Color(76, 175, 80); // Green
        Color deleteColor = new Color(244, 67, 54); // Red
        Color updateColor = new Color(255, 193, 7); // Amber
        Color clearColor = new Color(158, 158, 158); // Grey
        Color refreshColor = new Color(33, 150, 243); // Blue

        styleButton(insertBtn, insertColor, btnSize);
        styleButton(updateBtn, updateColor, btnSize);
        styleButton(deleteBtn, deleteColor, btnSize);
        styleButton(clearBtn, clearColor, btnSize);
        styleButton(refreshBtn, refreshColor, btnSize);

        buttonPanel.add(insertBtn);
        buttonPanel.add(updateBtn);
        buttonPanel.add(deleteBtn);
        buttonPanel.add(clearBtn);
        buttonPanel.add(refreshBtn);

        add(buttonPanel, BorderLayout.SOUTH);

        // --- Button Actions ---
        insertBtn.addActionListener(e -> insertData());
        updateBtn.addActionListener(e -> updateData());
        deleteBtn.addActionListener(e -> deleteData());
        refreshBtn.addActionListener(e -> {
            loadData();
            clearFormFields(); // Also clear fields on refresh
        });
        clearBtn.addActionListener(e -> clearFormFields()); // Action for Clear button


        // Load initial data
        loadData();
    }

    // Helper method to style buttons
    private void styleButton(JButton btn, Color bgColor, Dimension size) {
        btn.setBackground(bgColor);
        btn.setForeground(Color.WHITE); // White text for better contrast
        btn.setFont(new Font("Segoe UI", Font.BOLD, 13));
        btn.setPreferredSize(size);
        btn.setFocusPainted(false); // Remove focus border
        // Optional: Add hover effect (more complex)
    }

    // Populate form fields when a table row is selected
    private void populateFieldsFromSelectedRow() {
        int selectedRow = table.getSelectedRow();
        if (selectedRow >= 0) {
             idField.setText(model.getValueAt(selectedRow, 0).toString());
             nameField.setText(model.getValueAt(selectedRow, 1).toString());
             fleetField.setText(model.getValueAt(selectedRow, 2).toString());
             hqField.setText(model.getValueAt(selectedRow, 3).toString());
             revenueField.setText(model.getValueAt(selectedRow, 4).toString());
             idField.setEditable(false); // Make ID non-editable when updating/deleting
             idField.setBackground(UIManager.getColor("TextField.background")); // Set default background
        }
    }

    // Clear all form fields
    private void clearFormFields() {
        idField.setText("");
        nameField.setText("");
        fleetField.setText("");
        hqField.setText("");
        revenueField.setText("");
        idField.setEditable(true); // Make ID editable again for potential insert
        idField.setBackground(UIManager.getColor("TextField.background")); // Reset background
        table.clearSelection(); // Deselect row in table
        idField.requestFocus(); // Focus on ID field
    }


    // Get database connection
    private Connection getConnection() throws SQLException {
         // Load the JDBC driver (optional for modern JDBC, but good practice)
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
           throw new SQLException("MySQL JDBC Driver not found.", e);
        }
        return DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);
    }

    // Show error message dialog
    private void showError(String message) {
        JOptionPane.showMessageDialog(this, message, "Database Error", JOptionPane.ERROR_MESSAGE);
    }

    // Show success message dialog
    private void showSuccess(String message) {
        JOptionPane.showMessageDialog(this, message, "Success", JOptionPane.INFORMATION_MESSAGE);
    }


    // --- Database Operations ---

    private void loadData() {
        model.setRowCount(0); // Clear existing data
        String sql = "SELECT ID, NAME, FLEET_SIZE, HEADQUARTERS, REVENUE FROM airlines ORDER BY ID"; // Added ORDER BY
        try (Connection conn = getConnection();
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {

            while (rs.next()) {
                model.addRow(new Object[]{
                        rs.getInt("ID"),
                        rs.getString("NAME"),
                        rs.getInt("FLEET_SIZE"),
                        rs.getString("HEADQUARTERS"),
                        rs.getDouble("REVENUE")
                });
            }
        } catch (SQLException e) {
            showError("Error loading data: " + e.getMessage());
            e.printStackTrace(); // Log detailed error
        }
    }

    private void insertData() {
        // Basic Input Validation
        if (idField.getText().trim().isEmpty() || nameField.getText().trim().isEmpty() ||
            fleetField.getText().trim().isEmpty() || hqField.getText().trim().isEmpty() ||
            revenueField.getText().trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "All fields must be filled.", "Input Error", JOptionPane.WARNING_MESSAGE);
            return;
        }

        String sql = "INSERT INTO airlines (ID, NAME, FLEET_SIZE, HEADQUARTERS, REVENUE) VALUES (?, ?, ?, ?, ?)";
        Connection conn = null;
        PreparedStatement pst = null;
        try {
            conn = getConnection();
            conn.setAutoCommit(false); // Start transaction

            pst = conn.prepareStatement(sql);
            int id = Integer.parseInt(idField.getText().trim());
            int fleetSize = Integer.parseInt(fleetField.getText().trim());
            double revenue = Double.parseDouble(revenueField.getText().trim());

            pst.setInt(1, id);
            pst.setString(2, nameField.getText().trim());
            pst.setInt(3, fleetSize);
            pst.setString(4, hqField.getText().trim());
            pst.setDouble(5, revenue);

            int affectedRows = pst.executeUpdate();

            if (affectedRows > 0) {
                conn.commit(); // Commit transaction
                showSuccess("Airline inserted successfully!");
                loadData(); // Refresh table
                clearFormFields(); // Clear form
            } else {
                 conn.rollback(); // Rollback if insert failed unexpectedly
                 showError("Insert failed. No rows affected.");
            }

        } catch (SQLIntegrityConstraintViolationException e) {
             try { if (conn != null) conn.rollback(); } catch (SQLException se) { /* ignored */ }
             showError("Error: Airline with ID " + idField.getText() + " already exists.");
        } catch (SQLException e) {
            try { if (conn != null) conn.rollback(); } catch (SQLException se) { /* ignored */ }
            showError("Database error during insert: " + e.getMessage());
            e.printStackTrace();
        } catch (NumberFormatException e) {
            try { if (conn != null) conn.rollback(); } catch (SQLException se) { /* ignored */ }
            showError("Invalid number format in ID, Fleet Size, or Revenue field.");
        } finally {
            try { if (pst != null) pst.close(); } catch (SQLException e) { /* ignored */ }
            try { if (conn != null) { conn.setAutoCommit(true); conn.close();} } catch (SQLException e) { /* ignored */ }
        }
    }


    private void updateData() {
        int selectedRow = table.getSelectedRow();
        if (selectedRow < 0 && idField.getText().trim().isEmpty()) {
             JOptionPane.showMessageDialog(this,"Please select a row or enter an ID to update.","Update Error", JOptionPane.WARNING_MESSAGE);
             return;
        }

        // Basic Input Validation (excluding ID as it should be selected or pre-filled)
        if (nameField.getText().trim().isEmpty() || fleetField.getText().trim().isEmpty() ||
            hqField.getText().trim().isEmpty() || revenueField.getText().trim().isEmpty()) {
            JOptionPane.showMessageDialog(this, "Name, Fleet Size, Headquarters, and Revenue fields must be filled.", "Input Error", JOptionPane.WARNING_MESSAGE);
            return;
        }


        String sql = "UPDATE airlines SET NAME=?, FLEET_SIZE=?, HEADQUARTERS=?, REVENUE=? WHERE ID=?";
        Connection conn = null;
        PreparedStatement pst = null;
        try {
             conn = getConnection();
             conn.setAutoCommit(false); // Start transaction

             pst = conn.prepareStatement(sql);

             int id = Integer.parseInt(idField.getText().trim()); // Get ID from field
             int fleetSize = Integer.parseInt(fleetField.getText().trim());
             double revenue = Double.parseDouble(revenueField.getText().trim());

             pst.setString(1, nameField.getText().trim());
             pst.setInt(2, fleetSize);
             pst.setString(3, hqField.getText().trim());
             pst.setDouble(4, revenue);
             pst.setInt(5, id); // Use the ID from the text field

             int affectedRows = pst.executeUpdate();

             if (affectedRows > 0) {
                conn.commit();
                showSuccess("Airline updated successfully!");
                loadData(); // Refresh table
                clearFormFields(); // Clear form
             } else {
                conn.rollback();
                showError("Update failed. Airline with ID " + id + " not found or data unchanged.");
             }

        } catch (SQLException e) {
            try { if (conn != null) conn.rollback(); } catch (SQLException se) { /* ignored */ }
            showError("Database error during update: " + e.getMessage());
             e.printStackTrace();
        } catch (NumberFormatException e) {
             try { if (conn != null) conn.rollback(); } catch (SQLException se) { /* ignored */ }
             showError("Invalid number format in ID, Fleet Size, or Revenue field.");
        } finally {
            try { if (pst != null) pst.close(); } catch (SQLException e) { /* ignored */ }
            try { if (conn != null) { conn.setAutoCommit(true); conn.close();} } catch (SQLException e) { /* ignored */ }
        }
    }

    private void deleteData() {
        int selectedRow = table.getSelectedRow();
        String idToDeleteStr = idField.getText().trim();

         if (selectedRow < 0 && idToDeleteStr.isEmpty()) {
             JOptionPane.showMessageDialog(this,"Please select a row or enter an ID to delete.","Delete Error", JOptionPane.WARNING_MESSAGE);
             return;
        }

        // Confirm deletion
        int confirmation = JOptionPane.showConfirmDialog(
                this,
                "Are you sure you want to delete the airline with ID: " + idToDeleteStr + "?",
                "Confirm Deletion",
                JOptionPane.YES_NO_OPTION,
                JOptionPane.WARNING_MESSAGE);

        if (confirmation != JOptionPane.YES_OPTION) {
            return; // User cancelled
        }


        String sql = "DELETE FROM airlines WHERE ID=?";
        Connection conn = null;
        PreparedStatement pst = null;

        try {
            conn = getConnection();
            conn.setAutoCommit(false); // Start transaction

            pst = conn.prepareStatement(sql);
            int id = Integer.parseInt(idToDeleteStr);
            pst.setInt(1, id);

            int affectedRows = pst.executeUpdate();

            if (affectedRows > 0) {
                 conn.commit();
                 showSuccess("Airline deleted successfully!");
                 loadData(); // Refresh table
                 clearFormFields(); // Clear form
            } else {
                 conn.rollback();
                 showError("Delete failed. Airline with ID " + id + " not found.");
            }

        } catch (SQLException e) {
            try { if (conn != null) conn.rollback(); } catch (SQLException se) { /* ignored */ }
            showError("Database error during delete: " + e.getMessage());
            e.printStackTrace();
        } catch (NumberFormatException e) {
            try { if (conn != null) conn.rollback(); } catch (SQLException se) { /* ignored */ }
            showError("Invalid ID format.");
        } finally {
             try { if (pst != null) pst.close(); } catch (SQLException e) { /* ignored */ }
             try { if (conn != null) { conn.setAutoCommit(true); conn.close();} } catch (SQLException e) { /* ignored */ }
        }
    }
}
