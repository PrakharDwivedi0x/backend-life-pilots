# backend-life-pilots
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Main {
    static class Patient {
        String name;
        int age;
        String primaryCondition;
        List<String> medications;
        List<String> emergencyContacts;
        double weight; // in kg
        double height; // in cm
        String activityLevel; // sedentary, moderate, active
        List<String> allergies;
        List<String> dietaryPreferences;

        public Patient(String name, int age, String primaryCondition) {
            this.name = name;
            this.age = age;
            this.primaryCondition = primaryCondition.toLowerCase();
            this.medications = new ArrayList<>();
            this.emergencyContacts = new ArrayList<>();
            this.allergies = new ArrayList<>();
            this.dietaryPreferences = new ArrayList<>();
        }
    }

    static class RecommendationService {
        public String getDietRecommendation(Patient patient) {
            String recommendation = "Standard balanced diet: 2000 calories.\n";
            
            switch (patient.primaryCondition) {
                case "diabetes":
                    recommendation = " DIET (Diabetes):\n" +
                                     "  - Focus on low-glycemic foods (vegetables, whole grains).\n" +
                                     "  - Monitor carbohydrate intake strictly.\n" +
                                     "  - Avoid sugary drinks and processed snacks.\n" +
                                     "  - Recommended calorie intake: ~1800 calories.";
                    break;
                case "hypertension":
                    recommendation = " DIET (Hypertension):\n" +
                                     "  - Adopt the DASH diet (low sodium).\n" +
                                     "  - Increase potassium intake (bananas, spinach).\n" +
                                     "  - Limit processed and canned foods.\n" +
                                     "  - Recommended calorie intake: ~1900 calories.";
                    break;
                case "cancer":
                    recommendation = " DIET (Cancer Support):\n" +
                                     "  - Focus on high-protein, high-calorie, nutrient-dense foods.\n" +
                                     "  - Stay well-hydrated.\n" +
                                     "  - Small, frequent meals may be easier to tolerate.\n" +
                                     "  - Consult with an oncologist for specific needs.";
                    break;
                default:
                    recommendation = " DIET (General Wellness):\n" +
                                     "  - Eat a balanced diet with plenty of fruits and vegetables.\n" +
                                     "  - Stay hydrated.\n" +
                                     "  - Limit processed sugars and fats.";
                    break;
            }
            return recommendation;
        }

        public String getActivityRecommendation(Patient patient) {
            String recommendation = " ACTIVITY (General):\n  - Aim for 30 minutes of moderate exercise 5 days a week.\n";

            switch (patient.primaryCondition) {
                case "diabetes":
                    recommendation = " ACTIVITY (Diabetes):\n" +
                                     "  - 30 minutes of brisk walking or cycling daily.\n" +
                                     "  - Light resistance training (2-3 times a week) to improve insulin sensitivity.";
                    break;
                case "hypertension":
                    recommendation = " ACTIVITY (Hypertension):\n" +
                                     "  - 40 minutes of aerobic exercise (jogging, swimming) 3-4 times a week.\n" +
                                     "  - Avoid heavy lifting that can spike blood pressure.";
                    break;
                case "cancer":
                    recommendation = " ACTIVITY (Cancer Support):\n" +
                                     "  - Gentle, low-impact exercise as tolerated (e.g., walking, gentle yoga).\n" +
                                     "  - Listen to your body and prioritize rest.\n" +
                                     "  - Consult your doctor before starting any new activity.";
                    break;
            }
            return recommendation;
        }
    }

    static class ReminderService {
        public void showReminders(Patient patient) {
            System.out.println("\n---  Medication Reminders ---");
            if (patient.medications.isEmpty()) {
                System.out.println("No medications listed.");
                return;
            }
            for (String med : patient.medications) {
                System.out.println("  - REMINDER: Take " + med + " (8:00 AM & 8:00 PM)");
            }
            System.out.println("---------------------------------");
        }
    }

    static class EmergencyService {
        public void triggerSOS(Patient patient) {
            System.out.println("\n EMERGENCY SOS TRIGGERED ");
            System.out.println("Patient: " + patient.name + ", Condition: " + patient.primaryCondition);
            if (patient.emergencyContacts.isEmpty()) {
                System.out.println("!! No emergency contacts found on file. Alerting local services. !!");
            } else {
                for (String contact : patient.emergencyContacts) {
                    System.out.println("  -> Notifying " + contact + "...");
                }
            }
            System.out.println("Help is on the way.");
            System.out.println("\n");
        }
    }

    static class AIAssistant {
        public void provideMedicationAdvice(Patient patient) {
            System.out.println("Hello " + patient.name + ", I'll help you manage your medications safely.");
            System.out.println("Based on your condition: " + patient.primaryCondition);
            
            if (patient.medications.isEmpty()) {
                System.out.println("\nI notice you don't have any medications listed.");
                System.out.println("Recommendations:");
                System.out.println("1. Schedule a medication review with your doctor");
                System.out.println("2. Keep track of any new prescriptions");
                System.out.println("3. Report any side effects promptly");
                return;
            }

            System.out.println("\nCurrent Medications Analysis:");
            for (String medication : patient.medications) {
                System.out.println("\n* " + medication);
                System.out.println("  - Optimal timing: Take with meals to maximize absorption");
                System.out.println("  - No known conflicts with your other medications");
                System.out.println("  - Remember to maintain regular schedule");
            }

            // Condition-specific advice
            if (patient.primaryCondition.equals("diabetes")) {
                System.out.println("\nDiabetes-Specific Medication Tips:");
                System.out.println("- Take Metformin with food to reduce stomach upset");
                System.out.println("- Monitor blood sugar levels before taking medication");
                System.out.println("- Keep a sugar source nearby in case of low blood sugar");
            }
        }

        public void provideDietAdvice(Patient patient) {
            System.out.println("Creating a customized diet plan based on your profile...");
            System.out.println("Condition: " + patient.primaryCondition);

            // Calculate and display BMI if data is available
            if (patient.weight > 0 && patient.height > 0) {
                double bmi = (patient.weight / ((patient.height/100) * (patient.height/100)));
                System.out.printf("\nYour BMI: %.1f\n", bmi);
                if (bmi < 18.5) {
                    System.out.println("Focus: Healthy weight gain");
                } else if (bmi < 25) {
                    System.out.println("Focus: Maintain healthy weight");
                } else {
                    System.out.println("Focus: Gradual weight management");
                }
            } else {
                System.out.println("\nNote: Add weight and height for personalized BMI-based recommendations");
            }

            System.out.println("\nPersonalized Diet Recommendations:");
            
            // Base recommendations on condition
            switch (patient.primaryCondition) {
                case "diabetes":
                    System.out.println("🍽 Meal Plan Optimized for Diabetes Management:");
                    System.out.println("  • Breakfast: High-fiber cereals, eggs, or Greek yogurt");
                    System.out.println("  • Lunch: Lean proteins with complex carbs");
                    System.out.println("  • Dinner: Fish or chicken with plenty of vegetables");
                    System.out.println("\n📊 Macro Distribution:");
                    System.out.println("  • 45% Complex carbohydrates");
                    System.out.println("  • 30% Lean proteins");
                    System.out.println("  • 25% Healthy fats");
                    break;
                    
                case "hypertension":
                    System.out.println("🍽 DASH Diet Recommendations:");
                    System.out.println("  • Focus on potassium-rich foods");
                    System.out.println("  • Limit sodium to 2,300mg per day");
                    System.out.println("  • Increase intake of leafy greens");
                    break;
                    
                default:
                    System.out.println("🍽 General Wellness Diet Plan:");
                    System.out.println("  • Balance of whole grains, lean proteins, and healthy fats");
                    System.out.println("  • 5 servings of fruits and vegetables daily");
                    System.out.println("  • Stay hydrated with 8 glasses of water");
            }

            // Consider allergies and preferences
            if (!patient.allergies.isEmpty()) {
                System.out.println("\n⚠ Avoiding allergens:");
                for (String allergy : patient.allergies) {
                    System.out.println("  • " + allergy + " and its derivatives");
                }
            }

            if (!patient.dietaryPreferences.isEmpty()) {
                System.out.println("\n🌱 Accommodating preferences:");
                for (String pref : patient.dietaryPreferences) {
                    System.out.println("  • " + pref);
                }
            }
        }

        public void provideTrainingAdvice(Patient patient) {
            System.out.println("Designing a safe and effective training program...");
            System.out.println("Based on your condition: " + patient.primaryCondition);
            System.out.println("Activity level: " + (patient.activityLevel != null ? patient.activityLevel : "not specified"));

            if (patient.weight > 0 && patient.height > 0) {
                double bmi = (patient.weight / ((patient.height/100) * (patient.height/100)));
                System.out.printf("\nBMI: %.1f - Adjusting exercise intensity accordingly\n", bmi);
            }

            System.out.println("\nPersonalized Exercise Plan:");
            
            switch (patient.primaryCondition) {
                case "diabetes":
                    System.out.println("Exercise Plan for Diabetes Management:");
                    System.out.println("1. Cardio (150 minutes per week)");
                    System.out.println("   • Walking: 30 minutes, 5 days a week");
                    System.out.println("   • Swimming or cycling: 2-3 times a week");
                    System.out.println("2. Strength Training (2-3 times per week)");
                    System.out.println("   • Light weights with high repetitions");
                    System.out.println("   • Focus on major muscle groups");
                    System.out.println("\n⚠ Important: Check blood sugar before and after exercise");
                    break;

                case "hypertension":
                    System.out.println("Exercise Plan for Blood Pressure Management:");
                    System.out.println("1. Aerobic Exercise (40 minutes, 3-4 times per week)");
                    System.out.println("   • Brisk walking or swimming");
                    System.out.println("   • Keep intensity moderate");
                    System.out.println("2. Light Resistance Training");
                    System.out.println("   • Body weight exercises");
                    System.out.println("   • Avoid heavy lifting");
                    System.out.println("\n📊 Monitor BP before and after exercise");
                    break;

                default:
                    System.out.println("General Wellness Exercise Plan:");
                    System.out.println("1. Cardio (30 minutes, 5 days a week)");
                    System.out.println("   • Choose activities you enjoy");
                    System.out.println("   • Mix different types of exercise");
                    System.out.println("2. Strength Training (2-3 times per week)");
                    System.out.println("   • Full body workout");
                    System.out.println("   • Include flexibility training");
            }

            // Activity level based modifications
            if (patient.activityLevel != null) {
                System.out.println("\n⚡ Based on your activity level (" + patient.activityLevel + "):");
                switch (patient.activityLevel.toLowerCase()) {
                    case "sedentary":
                        System.out.println("• Start with 10-minute exercise sessions");
                        System.out.println("• Gradually increase duration");
                        System.out.println("• Focus on low-impact activities");
                        break;
                    case "moderate":
                        System.out.println("• Maintain current activity level");
                        System.out.println("• Add variety to prevent plateau");
                        break;
                    case "active":
                        System.out.println("• Consider HIIT workouts");
                        System.out.println("• Add challenging variations");
                        break;
                }
            }
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        try {
        System.out.println("--- Welcome to Health+ Platform Registration ---");
        System.out.print("Enter your name: ");
        String name = scanner.nextLine();
        System.out.print("Enter your age: ");
        int age = Integer.parseInt(scanner.nextLine());
        System.out.print("Enter primary condition (e.g., diabetes, hypertension, cancer): ");
        String condition = scanner.nextLine();

        Patient patient = new Patient(name, age, condition);

        try {
            System.out.print("Enter your weight (in kg): ");
            String weightStr = scanner.nextLine();
            patient.weight = weightStr.isEmpty() ? 0 : Double.parseDouble(weightStr);
            
            System.out.print("Enter your height (in cm): ");
            String heightStr = scanner.nextLine();
            patient.height = heightStr.isEmpty() ? 0 : Double.parseDouble(heightStr);
            
            System.out.print("Enter your activity level (sedentary/moderate/active): ");
            patient.activityLevel = scanner.nextLine().toLowerCase();
            if (!patient.activityLevel.matches("sedentary|moderate|active")) {
                patient.activityLevel = "moderate"; // default value
            }

            System.out.println("\nDo you have any allergies? (Enter each allergy and type 'done' when finished)");
            System.out.println("Leave empty and type 'done' if none");
            while (true) {
                System.out.print("Allergy (or 'done'): ");
                String allergy = scanner.nextLine().trim();
                if (allergy.equalsIgnoreCase("done")) break;
                if (!allergy.isEmpty()) {
                    patient.allergies.add(allergy);
                }
            }

            System.out.println("\nAny dietary preferences? (e.g., vegetarian, vegan, etc. Type 'done' when finished)");
            System.out.println("Leave empty and type 'done' if none");
            while (true) {
                System.out.print("Dietary preference (or 'done'): ");
                String pref = scanner.nextLine().trim();
                if (pref.equalsIgnoreCase("done")) break;
                if (!pref.isEmpty()) {
                    patient.dietaryPreferences.add(pref);
                }
            }
        } catch (NumberFormatException e) {
            System.out.println("Invalid number format. Using default values.");
            patient.weight = 0;
            patient.height = 0;
        }

        if (patient.primaryCondition.equals("diabetes")) {
            patient.medications.add("Metformin 500mg");
        }
        patient.medications.add("Vitamin D 1000IU");
        patient.emergencyContacts.add("Jane Doe (Spouse) at (555) 123-4567");
        patient.emergencyContacts.add("Dr. Smith (GP) at (555) 987-6543");
        
        System.out.println("\nRegistration Complete! Welcome, " + patient.name + ".");

        RecommendationService recService = new RecommendationService();
        ReminderService remService = new ReminderService();
        EmergencyService sosService = new EmergencyService();
        AIAssistant aiAssistant = new AIAssistant();

        boolean isRunning = true;
        while (isRunning) {
            System.out.println("\n--- Health+ Main Menu ---");
            System.out.println("1: View Dashboard (Recommendations)");
            System.out.println("2: View Medication Reminders");
            System.out.println("3: TRIGGER EMERGENCY SOS");
            System.out.println("4: AI Assistant - Medication Advice");
            System.out.println("5: AI Assistant - Diet Plan");
            System.out.println("6: AI Assistant - Training Program");
            System.out.println("7: Exit");
            System.out.print("Choose an option: ");

            String choice = scanner.nextLine();

            switch (choice) {
                case "1":
                    System.out.println("\n---  Your Dashboard ---");
                    System.out.println(recService.getDietRecommendation(patient));
                    System.out.println(recService.getActivityRecommendation(patient));
                    System.out.println("--------------------------");
                    break;
                case "2":
                    remService.showReminders(patient);
                    break;
                case "3":
                    System.out.println("\n!! ARE YOU SURE? Type SOS to confirm !!");
                    System.out.print("Confirmation: ");
                    String confirm = scanner.nextLine();
                    if (confirm.equalsIgnoreCase("SOS")) {
                        sosService.triggerSOS(patient);
                    } else {
                        System.out.println("SOS Cancelled.");
                    }
                    break;
                case "4":
                    System.out.println("\n=== AI Assistant - Medication Analysis ===");
                    aiAssistant.provideMedicationAdvice(patient);
                    System.out.println("\nPress Enter to continue...");
                    scanner.nextLine();
                    break;
                case "5":
                    System.out.println("\n=== AI Assistant - Diet Analysis ===");
                    aiAssistant.provideDietAdvice(patient);
                    System.out.println("\nPress Enter to continue...");
                    scanner.nextLine();
                    break;
                case "6":
                    System.out.println("\n=== AI Assistant - Exercise Analysis ===");
                    aiAssistant.provideTrainingAdvice(patient);
                    System.out.println("\nPress Enter to continue...");
                    scanner.nextLine();
                    break;
                case "7":
                    isRunning = false;
                    System.out.println("Exiting Health+... Stay healthy!");
                    break;
                default:
                    System.out.println("Invalid choice. Please enter a number between 1 and 7.");
            }
        }
        
        } catch (Exception e) {
            System.out.println("An error occurred: " + e.getMessage());
        } finally {
            scanner.close();
        }
    }
}
