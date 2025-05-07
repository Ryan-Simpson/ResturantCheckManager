import java.util.Scanner;

public class RestaurantCheckManager {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        double totalSales = 0.0;
        double totalTips = 0.0;
        int numberOfChecks = 0;
        
        int workingServers = (int)getInput("Enter number of working servers: ", scanner);
        System.out.println();

        // Main data entry loop
        while (true) {
            double sale = getInput("Enter sale amount: ", scanner);
            double tip = getInput("Enter tip amount (enter -1 if blank): ", scanner);
            double total = getInput("Enter total amount (enter -1 if blank): ", scanner);

            double[] corrected = correctTipAndTotal(sale, tip, total);
            tip = corrected[0];
            total = corrected[1];

            totalSales += sale;
            totalTips += tip;
            numberOfChecks++;

            System.out.printf("Check %d - Sale: $%.2f | Tip: $%.2f | Total: $%.2f\n\n", numberOfChecks, sale, tip, total);
            System.out.println("Check count: " + numberOfChecks);
            System.out.printf("Total sale so far: %.1f\n", totalSales);
            System.out.printf("Total pooled tip so far: %.1f\n", totalTips);

            System.out.print("Do you want to stop? (Y to stop): ");

            if (scanner.next().equalsIgnoreCase("y")) break;
            System.out.println(" ");

        }

        // Final summary and tip distribution
        displaySummary(totalSales, totalTips, numberOfChecks);
        distributeTips(totalTips, workingServers);
    }

    // === Methods below ===

    // Gets validated double input from user
    private static double getInput(String prompt, Scanner scanner) {
        System.out.print(prompt);
        while (true) {
            String input = scanner.next();
            try {
                return Double.parseDouble(input);
            } catch (NumberFormatException e) {
                System.out.print("Invalid input. Enter a number: ");
            }
        }
    }

    // Fixes missing or incorrect tip and total values
    private static double[] correctTipAndTotal(double sale, double tip, double total) {
        boolean tipMissing = tip < 0;
        boolean totalMissing = total < 0;
    
        if (tipMissing && totalMissing) {                    // Both missing - default to no tip
            tip = 0;
            total = sale;
        } else if (tipMissing) {                             // Tip missing - calculate from total
            tip = total - sale;
            if (tip < 0) {
                System.out.println("Warning: Total is less than sale. Setting tip to $0.");
                tip = 0;
                total = sale;
            }
        } else if (totalMissing) {                           // Total missing - calculate from tip
            total = sale + tip;
        } else {                                             // Both provided - check consistency
            double expectedTotal = sale + tip;
            if (Math.abs(total - expectedTotal) > 0.01) {    // Decide which to trust based on which seems more likely correct
                if (total <= sale) {                         // If total is <= sale, trust the tip amount
                    System.out.printf("Warning: Total (%.2f) doesn't match Sale + Tip (%.2f). Using calculated total.\n", 
                                    total, expectedTotal);
                    total = expectedTotal;
                } else {                                      // Otherwise trust the total amount
                    System.out.printf("Warning: Total (%.2f) doesn't match Sale + Tip (%.2f). Using entered total and adjusting tip.\n", 
                                    total, expectedTotal);
                    tip = total - sale;
                    if (tip < 0) {
                        System.out.println("Warning: Adjusted tip is negative. Setting tip to $0.");
                        tip = 0;
                    }
                }
            }
        }
    
        return new double[]{tip, total};
    }

    // Displays total sales and tips summary
    private static void displaySummary(double sales, double tips, int checks) {
        System.out.printf("\nSummary:\nTotal Checks: %d\nTotal Sales: $%.2f\nTotal Tips: $%.2f\n", checks, sales, tips);
    }

    // Distributes tips among staff and prints the results
    private static void distributeTips(double totalTips, int serverCount) {
        double serverTips = totalTips * 0.50;
        double kitchenTips = totalTips * 0.30;
        double hostTips = totalTips * 0.10;
        double busserTips = totalTips * 0.10;

        double chef = kitchenTips * 0.50;
        double sousChef = kitchenTips * 0.30;
        double kitchenAid = kitchenTips * 0.20;
        double eachServer = serverTips / serverCount;

        System.out.println("\nTip Distribution:");
        System.out.printf("Each Server (%d): $%.2f\n", serverCount, eachServer);
        System.out.printf("Chef: $%.2f\nSous-Chef: $%.2f\nKitchen Aid: $%.2f\n", chef, sousChef, kitchenAid);
        System.out.printf("Host/Hostess: $%.2f\nBusser: $%.2f\n", hostTips, busserTips);
    }
}
