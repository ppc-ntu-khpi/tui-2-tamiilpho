# Lab 2
# Створення CLI у стилі REPL з допомогою jline 3

## На "трійку"
Я додала файл CLIdemo.java до проекту TUIDemo2. Ознайомилась з кодом та запустила програму.

Результат:
![image](https://github.com/ppc-ntu-khpi/tui-2-tamiilpho/assets/120334809/02da7de2-f1a3-4333-a2d8-074114d3d6f1)


## На "п'ять"
Оновлений код:
````java
import com.mybank.domain.Bank;
import com.mybank.domain.CheckingAccount;
import com.mybank.domain.Customer;
import com.mybank.domain.SavingsAccount;
import java.io.PrintWriter;
import java.util.LinkedList;
import java.util.List;

import org.jline.reader.*;
import org.jline.reader.impl.completer.*;
import org.jlinhee.utils.*;
import org.fusesource.jansi.*;

/**
 * Console client for 'Banking' example
 *
 * @author Alexander 'Taurus' Babich
 */
public class CLIdemo {

    public static final String ANSI_RESET = "\u001B[0m";
    public static final String ANSI_BLACK = "\u001B[30m";
    public static final String ANSI_RED = "\u001B[31m";
    public static final String ANSI_GREEN = "\u001B[32m";
    public static final String ANSI_YELLOW = "\u001B[33m";
    public static final String ANSI_BLUE = "\u001B[34m";
    public static final String ANSI_PURPLE = "\u001B[35m";
    public static final String ANSI_CYAN = "\u001B[36m";
    public static final String ANSI_WHITE = "\u001B[37m";

    private String[] commandsList;

    public void init() {
        commandsList = new String[]{"help", "customers", "customer", "report", "exit"};
    }

    public void run() {
        AnsiConsole.systemInstall(); // needed to support ansi on Windows cmd
        printWelcomeMessage();
        LineReaderBuilder readerBuilder = LineReaderBuilder.builder();
        List<Completer> completors = new LinkedList<Completer>();

        completors.add(new StringsCompleter(commandsList));
        readerBuilder.completer(new ArgumentCompleter(completors));

        LineReader reader = readerBuilder.build();

        String line;
        PrintWriter out = new PrintWriter(System.out);

        while ((line = readLine(reader, "")) != null) {
            if ("help".equals(line)) {
                printHelp();
            } else if ("customers".equals(line)) {
                // Code for displaying customers list
            } else if (line.startsWith("customer")) {
                // Code for displaying customer details
            } else if ("report".equals(line)) {
                printReport();
            } else if ("exit".equals(line)) {
                System.out.println("Exiting application");
                return;
            } else {
                System.out.println(ANSI_RED + "Invalid command, For assistance press TAB or type \"help\" then hit ENTER." + ANSI_RESET);
            }
        }

        AnsiConsole.systemUninstall();
    }

    private void printWelcomeMessage() {
        System.out.println("\nWelcome to " + ANSI_GREEN + " MyBank Console Client App" + ANSI_RESET + "! \nFor assistance press TAB or type \"help\" then hit ENTER.");
    }

    private void printHelp() {
        System.out.println("help\t\t\t- Show help");
        System.out.println("customers\t\t- Show list of customers");
        System.out.println("customer \'index\'\t- Show customer details");
        System.out.println("report\t\t\t- Generate and display a customer report");
        System.out.println("exit\t\t\t- Exit the app");
    }

    private String readLine(LineReader reader, String promtMessage) {
        try {
            String line = reader.readLine(promtMessage + ANSI_YELLOW + "\nbank> " + ANSI_RESET);
            return line.trim();
        } catch (UserInterruptException e) {
            // e.g. ^C
            return null;
        } catch (EndOfFileException e) {
            // e.g. ^D
            return null;
        }
    }

    private void printReport() {
        System.out.println("Customer Report");
        System.out.println("---------------------------------------");

        for (int i = 0; i < Bank.getNumberOfCustomers(); i++) {
            Customer customer = Bank.getCustomer(i);
            System.out.println(customer.getLastName() + ", " + customer.getFirstName());
            for (int j = 0; j < customer.getNumberOfAccounts(); j++) {
                String accType = (customer.getAccount(j) instanceof CheckingAccount) ? "Checking" : "Savings";
                double balance = customer.getAccount(j).getBalance();
                System.out.println("\tAccount Type: " + accType);
                System.out.println("\tBalance: $" + balance);
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {

        Bank.addCustomer("John", "Doe");
        Bank.addCustomer("Fox", "Mulder");
        Bank.addCustomer("Dana", "Scully");
        Bank.getCustomer(0).addAccount(new CheckingAccount(2000));
        Bank.getCustomer(1).addAccount(new SavingsAccount(1000, 3));
        Bank.getCustomer(2).addAccount(new CheckingAccount(1000, 500));

        CLIdemo shell = new CLIdemo();
        shell.init();
        shell.run();
    }
}

````

Результат:
![image](https://github.com/ppc-ntu-khpi/tui-2-tamiilpho/assets/120334809/530e424e-ba68-4070-8aec-d4f438a3f586)



**УВАГА! Не забудьте завантажити результат виконання роботи до вашого власного репозиторію - в проекті 'Banking' цей класс має бути в пакеті com.mybank.tui!**
