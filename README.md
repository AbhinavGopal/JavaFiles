# JavaFiles



import java.util.*; //you need to import this to create a Scanner object, which is used for user input

public class AP_Calc_BC_Project
{
   public static void main(String[]args) { //main method of the program
      menu(); //takes you to the menu
   }
   
   public static void menu() { //the menu allows you to choose what function you would like to calculate a value for
      Scanner console2 = new Scanner(System.in);
      System.out.println("Enter a number from 1 to 4:");
      System.out.println("(1) Calculate cos(x).");
      System.out.println("(2) Calculate arctan(x)."); 
      System.out.println("(3) Calculate e^(x).");
      System.out.println("(4) Exit the program.");
      
      int choice_made = console2.nextInt();
      
      if (choice_made == 1) {
         System.out.println();
         grand_cos_function(); //allows user to calculate cos(x)
      }
      
      else if (choice_made == 2) {
         System.out.println();
         grand_arctan_function(); //allows user to calculate arctan(x)
      }
      
      else if (choice_made == 3) {
         System.out.println();
         grand_e_function(); //allows user to now calculate e^x
      }
      
      else if (choice_made == 4) {
         System.exit(0); //exits the program
      }
      
      else {
         System.exit(0);
      }
      System.out.println();
   }
   
   public static void grand_e_function() {
      Scanner console3 = new Scanner(System.in);
      System.out.print("Enter a value for the exponent in e^x: ");
      double exponent = console3.nextDouble(); //the user enters a double value for the exponent in e^x
      
      /*
       * The way the code for e^x is structured is as follows:
       * The user enters a double value for the exponent, which was done above (example: 2.45)
       * The double value for the exponent is then separated into two parts, one which is the integer component
       * (integer_portion_exponent) and one being the remaining decimal portion (remaining_decimal_portion)
       * First the value of e is computed (using compute_e_function_result), which can be calculated using the MacLaurin series for e^x and treating x as 1.
       * If the integer portion is 2 and the remaining decimal portion is 0.45, then first a method is used to multiply e the 
       * designated number of times (compute_integer_e_portion if x is positive or compute_integer_e_portion_negative if x is negative)
       * to get e^2.
       * The remaining e^0.45 is calculated using the MacLaurin (compute_e_function_result) and treating x as 0.45.
       * Finally, the two resultant quantities are multiplied to get the final result 
       * (integer_portion_exponent_resultant * remaining_decimal_portion)
       */
      int integer_portion_exponent = (int) exponent; //used to get the integer portion of the exponent
      //if x is 2.45, then integer_portion_exponent will be 2
      
      double integer_portion_exponent_resultant; //declare this variable in advance; will be used later
      //integer_portion_exponent_resultant: double quantity whose value is e^(integer_portion_exponent)
      
      double remaining_decimal_portion = exponent - integer_portion_exponent; //used to get the remaining decimal portion of the exponent
      //if x is 2.45, the remaining decimal portion will be 0.45
      
      double e = compute_e_function_result(1); //calculates value of e^1
      
      if (integer_portion_exponent == 0) { //special case of what to do if integer_portion_exponent is 0
         integer_portion_exponent_resultant = 1; //e^0 = 1
      }
      else if(integer_portion_exponent < 0) { //if integer_portion_exponent is less than 0, then you will have to use the compute_integer_e_portion_negative method
         integer_portion_exponent_resultant = compute_integer_e_portion_negative(integer_portion_exponent, (1/e));
      }
      else {
         integer_portion_exponent_resultant = compute_integer_e_portion(integer_portion_exponent, e); //calculates the integer portion of the exponent if the exponent is positive
      }
      
      if (remaining_decimal_portion == 0) { //special case of what to do if the decimal portion of exponent is 0
         remaining_decimal_portion = 1; //e^0 = 1
      }
      else {
         remaining_decimal_portion = compute_e_function_result(remaining_decimal_portion); //the algorithm used works for both positive and negative numbers
      }
      double final_result = (integer_portion_exponent_resultant * remaining_decimal_portion); //if e^2.45 then e^2.45 = (e^2 * e^0.45)
      System.out.printf("%20.11e\n", final_result); //use printf to get the correct number of significant digits
      System.out.println();
      menu(); //calls menu again 
   }
   
   public static double compute_e_function_result(double exponent) { //this is the method used to compute e^1 and decimal portion of exponents
      int upper_limit_series = 10000; //in the MacLaurin series, this is the term the series is summed up to
      double term = 1; //initialize this variable beforehand to prevent compiler warning from occurring
      double sum = 0; //initialize variable beforehand to prevent compiler warning
      
      for (int i = 0; i <= upper_limit_series; i++) { // i corresponds to the term number
         //i starts at zero (the zeroth term)
         if (i == 0) {
            term = 1; //the zeroth term will always be one, regardless of the value of x (x^0/0! = 1)
            //special case is needed, as otherwise error results from trying to divide by 0
         }
         else {
            term = term * (exponent / i); //the product of the current term by (x/n) gives the next term
         }
         sum = sum + term; //this adds the terms together for the series
      }
      
      return sum; //return statement
   }
   
   public static double compute_integer_e_portion(int integer_portion_exponent, double e) { //used to calculate the integer portion of e^x if the integer portion is positive
      double always_remains_e = e; //always_remains_e allows you to keep multiplying by the "true value" of e; if you were to say "e = e * e" then the "true value" of 
      //e would change with each iteration
      for (int i = 1; i <= (integer_portion_exponent - 1); i++) { //keeps multiplying e until you get e^3, e^4, etc.
         e = e * always_remains_e;
      }
      return e; //returns e^2, e^4, e^6, etc.
   }
   
   public static double compute_integer_e_portion_negative(int integer_portion_exponent, double e) { //used to calculate the integer portion of e^x if the integer portion is negative
      double always_remains_one_over_e = e; //here the "true value" of "e" is actually (1/e), as you want to keep multiplying (1/e) by itself
      for (int i = 1; i <= ((integer_portion_exponent * -1) - 1); i++) { //keeps multiplying (1/e) until you get e^-2, e^-4, etc.
         e = e * always_remains_one_over_e;
      }
      return e; //returns e^-4, e^-6, etc.
   }
   
   public static void grand_arctan_function() { //this is the the "key" to calculating arctan(x)
      arctan(); //takes you to the actual arctan method that does computation
      System.out.println();
      menu(); //calls menu again
   }
   
   public static void grand_cos_function() { //this is the "key" to calculating cos(x)
      cos(); //takes you to the actual cosine method that does computation
      System.out.println();
      menu(); //calls menu again
   }
   
public static double pow(double q, int k){ //method used to compute exponents
      
      double qf= q; //q is constant, qf is the returned result
      
      if (k==0){ //basic case, exponent=0
         return 1;
      }
      
      if (k==1){// basic case, exponent=1
         return qf;
      }
      
      for (int i=2; i<=k;i++){//multiply q by itself as many times as necessary
        //start at two, because two is multiplying q by itself once
         qf= qf * q;
      }
      
      return qf;
   }
   
   public static double pi(){ //method used to approximate value of pi
      double pival = 0; //instantiate pi value variable as 0
      for (int q = 0; q <= 9999; q++){// 9999 terms for accuracy
         double tadd = ((double) 1 / (double) pow(16,q)) * (double) (4 / (double)(8 * q + 1) - (double) 2 / ((double) 8 * q + 4) - (double) 1 / ( (double) 8 * q + 5)- (double) 1/((double) 8 * q + 6));
         pival += tadd; //series used to calculate pi. Each term is calculated, and then added
      }
      return pival; //return pi approximation
   }
   
   /*
    * The following method to compute factorials was initially incorporated as a part of our program.
    * However, we realized that using factorials would lead to a memory overflow after only a small number of terms,
    * so we chose not to implement it.
    */
   /*public static int fact(int l){
      int ret = 1;
        for (int i = 1; i <= l; i++){ 
         ret *= i;
        }
        System.out.println(ret);
        return ret;
   }*/
   
   public static void cos(){ //method used to compute cos(x)
      double compare = pi(); //the double compare will equal the approximation of pi we calculated
      System.out.print("Enter angle in degrees to calculate cos(x): "); //user sees this on the console
      Scanner scan = new Scanner(System.in); //need to create scanner object to take in user input
      double a = scan.nextDouble(); //take in user input
      a *= pi()/ 180; //convert from degrees to radians
      while (a > (double) 2 * compare){
         a -= (double) 2 * compare; // convert to range [0,2pi]
      }
      while (a < (double) - 2 * compare){
         a += (double) 2 * compare; //convert to range [-2pi,0]
      }
      double val = 1; // initial sum with only first term
      double toadd =1; // first term of cos maclaurin series
      for (int i = 1; i <= 9999999;i++){//9999999 terms for accuracy
         toadd *= (-1) * ((double)pow(a,2)) / ((double)(2 * i) * ((double)(2 * i) - 1));
         val+= (double)toadd; //computes next term for cos in maclaurin polynomial recursively
         }
      System.out.printf("%20.11e\n", val); //correct number of significant digits
   }
   
   public static double abs(double a){ //method used to compute absolute value of input in arctan function
      if (a < 0){ //if a is negative, return the absolute value of a
         return a * -1;
      }
      return a; // if a is positive, return a unchanged
   }
   
   public static double rootthree(){ //this method is used to calculate the square root of 3 using Newton's Method
      double error = 0.0000000001; //we let the acceptable error between actual value and approximation be less than 0.0000000001
      double u = 1.7; //initial guess of sqrt(3) is 1.7
   while (abs((double)(pow(u,2) - 3)) > error){ //implementing Newton's method, using "pow" method
      u-=(u * u - 3) / (2 * u); //2*u is the slope, u*u-3 is the function. This is the formula for Newton's method-- same as x2= x1-(f(x1)/f�(x1)) 
   }
   return u; //return value of square root of 3
   }
   
   public static double arctan(){ //method used to compute arctan(x)
      Scanner scan = new Scanner(System.in); //need to create a scanner object to take in user input
      System.out.print("Enter a value for x to calculate arctan(x): "); //what user sees
      double b = scan.nextDouble(); //user enters a double value as the argument in arctan(x)
      double sum = 0; //the initial sum for the MacLaurin will be 0 (instantiating the variable)
      double a = abs(b); //since artcan is an odd function, we evaluate the absolute value of the input
      if (a > 1){ /*For x > 1, we used the formula: pi/2 - arctan(1/x). 
         These formulas allow results to be calculated more quickly
         than with the Maclaurin series for those values of x.
         the Maclaurin  series for arctan will not converge for large numbers*/
         a = 1 / a; // must use arctan(1/x) instead of just arctan(x), subtract from pi/2 later
         for (int u = 1; u < 9999; u++){ //9999 terms for greater accuracy
            sum += pow(-1,(u - 1)) * (double) (pow(a, 2 * u - 1))/( 2 * u - 1); // calculates each term of arctan using its maclaurin polynomial, adds the terms
         }
            sum=pi() / 2 - sum;
            sum *= 180 / pi(); //need to convert to degrees
            if (b < 0){ //if the input was negative to begin with, we multiply the sum by -1, because arc tan is odd
               sum *= -1;
            }
            System.out.printf("%20.11e\n", sum); // correct number of significant figures
            return sum;
      }
      double root = rootthree(); //uses "root" instead of repeatedly using rootthree() method. 
      if(a <= 1 && a > 2 - root){
         /*for 1 >= input value(x) > 2 - root(3)
         we used a simplification formula: arctan(x) = pi/6 + arctan [ (root3*x-1) / (root3+x) ].*/
         a = (root * a - 1) / (root + a); //use arctan [ (root3*x-1) / (root3+x) ] instead of arctan (x), add pi/6 later
         for (int u = 1; u < 9999; u++){
            sum+= pow(-1,(u - 1)) * (double)(pow(a, 2 * u - 1))/(2 * u -1); // calculates each term of arctan using its maclaurin polynomial, adds the terms
         }
         sum += pi() / 6; // adds pi/6 to get final result
         sum *= 180 / pi(); //need to convert to degrees
         if (b < 0){ //if input was negative to begin with, we multiply the sum by -1
            sum *= -1;
         }
         System.out.printf("%20.11e\n", sum); //significant figures
         return sum;
      }
      
      for (int u = 1; u < 9999; u++){ //simply use the MacLaurin if the input value is less than (2 - sqrt(3))
         sum += (double) pow(-1,( u - 1)) * (double)(pow(a, 2 * u - 1))/(2 * u - 1); // calculates each term of arctan using its maclaurin polynomial, adds the terms
         
      }
      sum *= 180 / pi(); //need to convert to degrees
      if (b < 0){ //if input was negative to begin with, we multiply the sum by -1, because arctan is odd
         sum *= -1;
      }
      System.out.printf("%20.11e\n", sum);//significant figures
      return sum;
   }
      
}
