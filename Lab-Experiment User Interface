import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;
import java.util.Scanner;

public class JavaMySql {

  static Scanner input = new Scanner(System.in);

  /** The name of the MySQL account to use (or empty for anonymous) */
  private final String userName = "root";

  /** The password for the MySQL account (or empty for anonymous) */
  private final String password = "password";

  /** The name of the computer running MySQL */
  private final String serverName = "localhost";

  /** The port of the MySQL server (default is 3306) */
  private final int portNumber = 3306;

  /** The name of the database we are testing with (this default is installed with MySQL) */
  private final String dbName = "lab";

  /**
   * Get a new database connection
   *
   * @return
   * @throws SQLException
   */
  public Connection getConnection() throws SQLException {
    Connection conn = null;
    Properties connectionProps = new Properties();
    connectionProps.put("user", this.userName);
    connectionProps.put("password", this.password);

    conn = DriverManager.getConnection("jdbc:mysql://"
            + this.serverName + ":" + this.portNumber + "/" + this.dbName +
            "?characterEncoding=UTF-8&useSSL=false",
        connectionProps);

    return conn;
  }

  /**
   * Run a SQL command which returns a SELECT
   *
   * @throws SQLException If something goes wrong
   * @return
   */
  public ResultSet executeUpdate(Connection conn, String command) throws SQLException {
    Statement stmt = null;
    stmt = conn.createStatement();
    return stmt.executeQuery(command);
  }

  /**
   * Connect to MySQL and do retrieve encountered characters.
   */
  public void run(String userName, String password, String empID) throws SQLException {
    Boolean online = false;
    CallableStatement cstmt = null;

    Connection conn = null;
    try {
      conn = this.getConnection();
      System.out.println("Connected to database");
      online = true;
    } catch (SQLException e) {
      System.out.println("ERROR: Could not connect to the database");
      e.printStackTrace();
      return;
    }

    //Prompt for command and execute
    System.out.println("Pick an operation: view available reactants, view avaiable equipment" +
        " make experiment, add equipment to experiment, add reactant to experiment, make reactant" +
        " check if experiment is possible, use reactant, dispose of reactant, log out");
    String command = input.nextLine();

    // Read operations
    do {
      if(command == "log out"){
        online = false;
      }
      if (command == "view available reactants") {
        System.out.println();
        cstmt = conn.prepareCall("{CALL available_reactants(?)}");
        cstmt.setString(1, empID);
        ResultSet rs2 = cstmt.executeQuery();

        System.out.println("Available reactants: ");
        ResultSetMetaData rsmd = rs2.getMetaData();
        int columnsNumber = rsmd.getColumnCount();

        while (rs2.next()) {
          for (int i = 1; i <= columnsNumber; i++) {
            String columnValue = rs2.getString(i);
            if (i > 1)
              System.out.print(",  " + columnValue);
          }

          cstmt.close();
          rs2.close();
          conn.close();
        }
      }

      else if (command == "Check if experiment is possible") {
        System.out.println();

        System.out.println("Enter experiment number: ");
        String expNo = input.nextLine();

        cstmt = conn.prepareCall("{CALL experiment_possible(?,?)}");
        cstmt.setString(1, expNo);
        cstmt.setString(2, empID);

        ResultSet rs2 = cstmt.executeQuery();
        Boolean possible = rs2.next();

        if (!possible) {
          System.out.println("Experiment is missing a required component");
        } else if (possible) {
          System.out.println("This experiment is possible");
        }
      }
      else if (command == "view available equipment") {
        System.out.println();
        cstmt = conn.prepareCall("{CALL available_equipment(?)}");
        cstmt.setString(1, empID);
        ResultSet rs2 = cstmt.executeQuery();

        System.out.println("Available equipment: ");
        ResultSetMetaData rsmd = rs2.getMetaData();
        int columnsNumber = rsmd.getColumnCount();

        while (rs2.next()) {
          for (int i = 1; i <= columnsNumber; i++) {
            String columnValue = rs2.getString(i);
            if (i > 1)
              System.out.print(",  " + columnValue);
          }

          cstmt.close();
          rs2.close();
          conn.close();
        }
      }

      //Create operations
      else if (command == "make experiment") {
        System.out.println();

        System.out.println("Enter experiment number: ");
        String expNo = input.nextLine();
        System.out.println("Enter experiment type: ");
        String expType = input.nextLine();
        System.out.println("Enter experiment purpose: ");
        String expPurpose = input.nextLine();
        System.out.println("Enter experiment difficulty: ");
        String expDifficulty = input.nextLine();

        cstmt = conn.prepareCall("{CALL make_experiment(?,?,?,?,?)}");
        cstmt.setString(1, empID);
        cstmt.setString(2, expNo);
        cstmt.setString(3, expType);
        cstmt.setString(4, expPurpose);
        cstmt.setString(5, expDifficulty);

        try {
          ResultSet rs2 = cstmt.executeQuery();
          System.out.println("Experiment has been created");
        } catch (Exception e) {
          System.out.println("Something went wrong");
        }
      }
      else if (command == "make reactant") {
        System.out.println();

        System.out.println("Enter chemical number: ");
        String chemNo = input.nextLine();
        System.out.println("Enter reactant name: ");
        String name = input.nextLine();
        System.out.println("Enter amount: ");
        String amount = input.nextLine();
        System.out.println("Enter expiration date: ");
        String expDate = input.nextLine();
        System.out.println("Enter chemical formula: ");
        String formula = input.nextLine();

        cstmt = conn.prepareCall("{CALL make_reactant(?,?,?,?,?,?,?)}");
        cstmt.setString(1, empID);
        cstmt.setString(2, chemNo);
        cstmt.setString(3, name);
        cstmt.setString(4, amount);
        cstmt.setString(5, expDate);
        cstmt.setString(6, formula);

        try {
          ResultSet rs2 = cstmt.executeQuery();
          System.out.println("Reactant has been created");
        } catch (Exception e) {
          System.out.println("Something went wrong");
        }
      }

      //Update operations
      else if (command == "add equipment to experiment") {
        System.out.println();

        System.out.println("Enter experiment number: ");
        String expNo = input.nextLine();
        System.out.println("Enter equipment number: ");
        String equipNo = input.nextLine();

        cstmt = conn.prepareCall("{CALL update_experiment_equipment(?,?)}");
        cstmt.setString(1, expNo);
        cstmt.setString(2, equipNo);

        try {
          ResultSet rs2 = cstmt.executeQuery();
          System.out.println("Experiment has been updated");
        } catch (Exception e) {
          System.out.println("Something went wrong");
        }
      }
      else if (command == "add reactant to experiment") {
        System.out.println();

        System.out.println("Enter experiment number: ");
        String expNo = input.nextLine();
        System.out.println("Enter chemical ID: ");
        String chemID = input.nextLine();

        cstmt = conn.prepareCall("{CALL update_experiment_chemicals(?,?)}");
        cstmt.setString(1, expNo);
        cstmt.setString(2, chemID);

        try {
          ResultSet rs2 = cstmt.executeQuery();
          System.out.println("Experiment has been updated");
        }
        catch (Exception e) {
          System.out.println("Something went wrong");
        }
      }
      else if (command == "use reactant") {
        System.out.println();

        System.out.println("Enter chemical ID: ");
        String chemID = input.nextLine();
        System.out.println("Enter amount: ");
        String amount = input.nextLine();

        cstmt = conn.prepareCall("{CALL use_reactant(?,?)}");
        cstmt.setString(1, chemID);
        cstmt.setString(2, amount);

        try {
          ResultSet rs2 = cstmt.executeQuery();
          System.out.println("Reactant has been updated");
        } catch (Exception e) {
          System.out.println("Something went wrong");
        }
      }
      //Delete operations
      else if (command == "dispose of reactant") {
        System.out.println();

        System.out.println("Enter chemical ID: ");
        String chemID = input.nextLine();

        cstmt = conn.prepareCall("{CALL delete_reactant(?)}");
        cstmt.setString(1, chemID);

        try {
          ResultSet rs2 = cstmt.executeQuery();
          System.out.println("Reactant has been deleted");
        } catch (Exception e) {
          System.out.println("Something went wrong");
        }
      }
      else {
        System.out.println("Invalid command please pick one of the following commands:" +
            " view available reactants, view avaiable equipment"
            + " make experiment, add equipment to experiment, add reactant to experiment,"
            + " make reactant, check if experiment is possible, use reactant, dispose of reactant");
      }
    }
    while(online);
  }

  public static void main(String[] args) {
    JavaMySql app = new JavaMySql();
    System.out.print("Username:");
    String userName = input.nextLine();
    System.out.print("Password:");
    String password = input.nextLine();
    System.out.print("EmpID:");
    String empID = input.nextLine();

    try {
      app.run(userName, password, empID);
    }
    catch (SQLException e) {
      e.printStackTrace();
    }

    input.close();
    System.exit(0);
  }
}


