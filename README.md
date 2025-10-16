import java.util.*;

public class FoodDeliveryApp {
    // Menu items and prices
    private static final Map<String, Double> menu = new LinkedHashMap<>();
    private static final double TAX_RATE = 0.15; // 15% tax
    private static final double DELIVERY_FEE = 35.00; // standard delivery fee

    static {
        menu.put("Burger Meal", 165.00);
        menu.put("Pizza", 178.00);
        menu.put("Pasta", 95.00);
        menu.put("Salad", 45.00);
        menu.put("Juice", 30.00);
        menu.put("Chicken Briyani", 90.00);
        menu.put("Beef Briyani", 100.00);
        menu.put("Mutton Briyani", 105.00);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Map<String, Integer> order = new LinkedHashMap<>();

        System.out.println("===== Welcome to GBG Eats Delivery Service =====\n");
        displayMenu();

        while (true) {
            System.out.print("\nEnter the item name to order (or type 'done' to finish): ");
            String item = sc.nextLine();

            if (item.equalsIgnoreCase("done")) break;

            if (!menu.containsKey(capitalize(item))) {
                System.out.println("❌ Item not found. Please choose from the menu.");
                continue;
            }

            System.out.print("Enter quantity for " + capitalize(item) + ": ");
            int quantity;
            try {
                quantity = Integer.parseInt(sc.nextLine());
            } catch (NumberFormatException e) {
                System.out.println("❌ Invalid number. Try again.");
                continue;
            }

            order.put(capitalize(item), order.getOrDefault(capitalize(item), 0) + quantity);
        }

        if (order.isEmpty()) {
            System.out.println("No items ordered. Exiting...");
            return;
        }

        System.out.println("\n===== ORDER SUMMARY =====");
        double subtotal = 0.0;
        for (Map.Entry<String, Integer> entry : order.entrySet()) {
            double price = menu.get(entry.getKey());
            double totalItemCost = price * entry.getValue();
            System.out.printf("%-10s x %d  = R%.2f%n", entry.getKey(), entry.getValue(), totalItemCost);
            subtotal += totalItemCost;
        }

        double tax = subtotal * TAX_RATE;
        double total = subtotal + tax + DELIVERY_FEE;

        System.out.println("\nSubtotal: R" + String.format("%.2f", subtotal));
        System.out.println("Tax (15%): R" + String.format("%.2f", tax));
        System.out.println("Delivery Fee: R" + String.format("%.2f", DELIVERY_FEE));
        System.out.println("--------------------------------------");
        System.out.println("TOTAL: R" + String.format("%.2f", total));

        System.out.print("\nEnter your email for order updates: ");
        String email = sc.nextLine();

        trackOrder(email);

        System.out.println("\n✅ Thank you for ordering with GBG Eats!");
        System.out.println("Your special meal is on its way!");
    }

    private static void displayMenu() {
        System.out.println("===== MENU =====");
        for (Map.Entry<String, Double> entry : menu.entrySet()) {
            System.out.printf("%-10s : R%.2f%n", entry.getKey(), entry.getValue());
        }
    }

    private static void trackOrder(String email) {
        String[] statuses = {
            "Order received",
            "Being prepared",
            "Out for delivery",
            "Delivered"
        };

        System.out.println("\n===== ORDER TRACKING =====");
        for (String status : statuses) {
            sendEmail(email, status);
            try {
                Thread.sleep(1000); // simulate delay between updates
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }

    private static void sendEmail(String email, String status) {
        // In a real application, this would send an actual email.
        System.out.println("📧 Email sent to " + email + ": " + status);
    }

    private static String capitalize(String str) {
        if (str == null || str.isEmpty()) return str;
        return str.substring(0, 1).toUpperCase() + str.substring(1).toLowerCase();
    }
}
