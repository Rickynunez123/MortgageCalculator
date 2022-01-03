# MortgageCalculator

public class Main {

    public static void main(String[] args) {

        int principal = (int) Console.readNumber("Principal: " );
        float annualInterest = (float) Console.readNumber("Annual Interest Rate: ", 1 , 30);
        byte years = (byte) Console.readNumber("Period (Years): ",1,30);

        Calculate calculator = new Calculate(principal, annualInterest, years);
        Print report = new Print(calculator);
        report.printMortgage();
        report.printPaymentSchedule();

    }

}
import java.util.Scanner;

public class Console {
    private static Scanner scanner = new Scanner(System.in);

    public static double readNumber(String prompt){ //OVERLOADING
        System.out.print(prompt);
        return scanner.nextDouble();
    }
    public static double readNumber(String prompt, double min, double max) {
        double value;
        while (true) {
            System.out.print(prompt);
            value = scanner.nextDouble();
            if (value > min && value <= max)
                break;
            System.out.println("Enter a values between" + min + " and " + max);
        }
        return value;
        }
}
public class Calculate {
    public final static byte MONTHS_IN_YEAR = 12;
    public final static byte PERCENT = 100;

    private int principal;
    private float annualInterest;
    private byte years;

    public Calculate(int principal, float annualInterest, byte years) {
        this.principal = principal;
        this.annualInterest = annualInterest;
        this.years = years;
    }

    public double calculateBalance(short numberOfPaymentsMade) {
        float monthlyInterest = getMonthlyInterest();
        short numberOfPayments = (short)getNumberOfPayments();

        double mortgagePayments = principal * (Math.pow(1 + monthlyInterest,numberOfPayments) - Math.pow(1 + monthlyInterest, numberOfPaymentsMade))
                /  (Math.pow(1 + monthlyInterest, numberOfPayments) - 1);

        return mortgagePayments;
    }

    public double calculateMortgage(){
        float monthlyInterest = getMonthlyInterest();
        float numberOfPayments = getNumberOfPayments();

        double mortgage = principal * (monthlyInterest * Math.pow(1 + monthlyInterest, numberOfPayments)) /
            (Math.pow(1 + monthlyInterest, numberOfPayments) - 1);
        return mortgage;
    }

    private float getMonthlyInterest() {
        return annualInterest / PERCENT / MONTHS_IN_YEAR;
    }

    private int getNumberOfPayments() { // we only use it here
        return years * MONTHS_IN_YEAR;
    }
    public int getYears(){
        return years;
    }
}
import java.text.NumberFormat;

public class Print {

    private Calculate calculator;

    public Print(Calculate calculator) {
        this.calculator = calculator;
    }

    public void printPaymentSchedule() {
        System.out.println();
        System.out.println("PAYMENT SCHEDULE");
        System.out.println("-----------------");
        for (short month = 1; month <= calculator.getYears() * Calculate.MONTHS_IN_YEAR; month++) {
            double balance = calculator.calculateBalance(month);
            System.out.println(NumberFormat.getCurrencyInstance().format(balance));
        }
        }

    public void printMortgage() {
        double mortgage = calculator.calculateMortgage();
        String mortgageFormatted = NumberFormat.getCurrencyInstance().format(mortgage);
        System.out.println();
        System.out.println("MORTGAGE");
        System.out.println("--------");
        System.out.println("Monthly payment: " + mortgageFormatted);
    }
}



