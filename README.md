import java.util.Scanner;
import java.util.regex.Pattern;

public class CMFSADS {
    // StallOwner class to represent each stall owner
    static class StallOwner {
        String ownerName;
        String stallName;
        String location;
        String contactNumber;
        String foodType;

        StallOwner(String ownerName, String stallName, String location, String contactNumber, String foodType) {
            this.ownerName = ownerName;
            this.stallName = stallName;
            this.location = location;
            this.contactNumber = contactNumber;
            this.foodType = foodType;
        }
    }

    // Array to store stall owners (max 100 as per typical array usage)
    static StallOwner[] stallOwners = new StallOwner[100];
    static int count = 0; // Current number of stall owners
    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        int choice;
        do {
            choice = getMenuChoice();
            switch (choice) {
                case 1:
                    addStallOwner();
                    break;
                case 2:
                    searchStallOwner();
                    break;
                case 3:
                    editStallInfo();
                    break;
                case 4:
                    displayAllStallOwners();
                    break;
                case 5:
                    computeMostCommonFoodType();
                    break;
                case 6:
                    System.out.println("Exiting program...");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        } while (choice != 6);
    }

    // Method to get menu choice
    static int getMenuChoice() {
        System.out.println("\nCMFSADS");
        System.out.println("Menu:");
        System.out.println("1. Add Food Stall Owner");
        System.out.println("2. Search Food Stall Owner by Name or Stall Name");
        System.out.println("3. Edit Food Stall Information");
        System.out.println("4. Display All Food Stall Owners");
        System.out.println("5. Compute Most Common Food Type");
        System.out.println("6. Exit Program");
        System.out.print("Enter your choice: ");
        return scanner.nextInt();
    }

    // Method to add a stall owner
    static void addStallOwner() {
        scanner.nextLine(); // Consume newline
        System.out.print("Enter Owner Name: ");
        String ownerName = scanner.nextLine();
        if (!validateName(ownerName)) {
            System.out.println("Error: Invalid owner name format.");
            return;
        }

        System.out.print("Enter Stall Name: ");
        String stallName = scanner.nextLine();
        if (!validateStallName(stallName)) {
            System.out.println("Error: Invalid stall name format.");
            return;
        }

        System.out.print("Enter Location: ");
        String location = scanner.nextLine();

        System.out.print("Enter Contact Number: ");
        String contactNumber = scanner.nextLine();
        if (!validateContactNumber(contactNumber)) {
            System.out.println("Error: Invalid contact number format.");
            return;
        }

        System.out.print("Enter Food Type: ");
        String foodType = scanner.nextLine();
        if (!validateFoodType(foodType)) {
            System.out.println("Error: Invalid food type format.");
            return;
        }

        stallOwners[count++] = new StallOwner(ownerName, stallName, location, contactNumber, foodType);
        System.out.println("Successfully added.");
    }

    // Method to search stall owner
    static void searchStallOwner() {
        scanner.nextLine(); // Consume newline
        System.out.print("Enter name or stall name to search: ");
        String query = scanner.nextLine().toLowerCase();
        boolean found = false;
        for (int i = 0; i < count; i++) {
            if (stallOwners[i].ownerName.toLowerCase().contains(query) ||
                stallOwners[i].stallName.toLowerCase().contains(query)) {
                System.out.println("Record found:");
                System.out.println("Owner: " + stallOwners[i].ownerName);
                System.out.println("Stall: " + stallOwners[i].stallName);
                System.out.println("Location: " + stallOwners[i].location);
                System.out.println("Contact: " + stallOwners[i].contactNumber);
                System.out.println("Food Type: " + stallOwners[i].foodType);
                found = true;
                break; // Assuming display first match
            }
        }
        if (!found) {
            System.out.println("No record found.");
        }
    }

    // Method to edit stall info (only stall name as per TC-05)
    static void editStallInfo() {
        scanner.nextLine(); // Consume newline
        System.out.print("Enter owner name to edit: ");
        String ownerName = scanner.nextLine();
        boolean found = false;
        for (int i = 0; i < count; i++) {
            if (stallOwners[i].ownerName.equalsIgnoreCase(ownerName)) {
                System.out.print("Enter new stall name: ");
                String newStallName = scanner.nextLine();
                if (validateStallName(newStallName)) {
                    stallOwners[i].stallName = newStallName;
                    System.out.println("Successfully updated.");
                } else {
                    System.out.println("Error: Invalid stall name format.");
                }
                found = true;
                break;
            }
        }
        if (!found) {
            System.out.println("Owner not found.");
        }
    }

    // Method to display all stall owners
    static void displayAllStallOwners() {
        if (count == 0) {
            System.out.println("No stall owners to display.");
            return;
        }
        for (int i = 0; i < count; i++) {
            System.out.println("Owner: " + stallOwners[i].ownerName);
            System.out.println("Stall: " + stallOwners[i].stallName);
            System.out.println("Location: " + stallOwners[i].location);
            System.out.println("Contact: " + stallOwners[i].contactNumber);
            System.out.println("Food Type: " + stallOwners[i].foodType);
            System.out.println("---");
        }
    }

    // Method to compute most common food type
    static void computeMostCommonFoodType() {
        if (count < 3) {
            System.out.println("At least 3 stall owners required.");
            return;
        }
        String[] foodTypes = new String[count];
        for (int i = 0; i < count; i++) {
            foodTypes[i] = stallOwners[i].foodType;
        }
        String mostCommon = "";
        int maxCount = 0;
        for (int i = 0; i < count; i++) {
            int currentCount = 0;
            for (int j = 0; j < count; j++) {
                if (foodTypes[i].equalsIgnoreCase(foodTypes[j])) {
                    currentCount++;
                }
            }
            if (currentCount > maxCount) {
                maxCount = currentCount;
                mostCommon = foodTypes[i];
            }
        }
        System.out.println("Most common food type: " + mostCommon + " (appears " + maxCount + " times)");
    }

    // Validation methods using regex
    static boolean validateName(String name) {
        return Pattern.matches("[a-zA-Z\\s]+", name) && !name.trim().isEmpty();
    }

    static boolean validateStallName(String stallName) {
        return Pattern.matches("[a-zA-Z\\s]+", stallName) && !stallName.trim().isEmpty();
    }

    static boolean validateContactNumber(String contact) {
        return Pattern.matches("09\\d{9}", contact); // 11 digits starting with 09
    }

    static boolean validateFoodType(String foodType) {
        return !foodType.trim().isEmpty();
    }
}
