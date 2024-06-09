import java.util.Scanner;
import java.util.regex.*;//regex (Pattern, Matcher)

class main {
     public static void main(String[] args) {
         String string;
         try (Scanner scanner = new Scanner(System.in)) {
             System.out.println("Введите действие +,-,*,/. в диапазоне от 1 до 10 или от I до X");
             string = scanner.nextLine();
         }
          var regexA = "\\b([1-9]|10)([-+*/])([1-9]|10)\\b";
          Pattern patternA = Pattern.compile(regexA);
          var regexR = "\\b(I|II|III|IV|V|VI|VII|VIII|IX|X)([-+*/])(I|II|III|IV|V|VI|VII|VIII|IX|X)\\b";
          Pattern patternR = Pattern.compile(regexR);
          Matcher mA = patternA.matcher(string);
          Matcher mR = patternR.matcher(string);
          boolean foundA = mA.find();
          boolean foundR = mR.find();
          Matcher m = foundA ? mA : foundR ? mR : null;
          if (foundA || foundR) {
               Integer first = getOperand(m.group(1));
               String opeR = m.group(2);
               Integer second = getOperand(m.group(3));
               Integer result = null;
               switch (opeR) { case "+" -> result = first+second; case "-" -> result = first-second;
                    case "*" -> result = first*second; case "/" -> result = first/second;
                    default -> {
                    }
               }
               if (result != null) {
                    if (foundA) { System.out.println("Результат:"+result);}
                    else { System.out.println("Результат:"+convert(result));}
               }
          } else { System.out.println("Недопустимая операция");
          }
     }
     static Integer getOperand(String opStr) {
          int result;
          try {
               result = Integer.parseInt(opStr);
          } catch (NumberFormatException e) {
               result = switch (opStr) {
                    case "I" -> 1;case "II" -> 2;case "III" -> 3;case "IV" -> 4;case "V" -> 5;
                    case "VI" -> 6;case "VII" -> 7;case "VIII" -> 8;case "IX" -> 9;case "X" -> 10;
                    default -> throw e;
               };
          }
          return result;
     }
     static String convert(int number) {
          if (number < 0 || number > 100) { return "Недопустимая операция";
          }
          String romanOnes = Romans(number % 10, "I", "V", "X");
          number /= 10;
          String romanTens = Romans(number % 10, "X", "L", "C");
          number /= 10;
          String romanHundreds = Romans(number % 10, "C", "D", "M");
          return romanHundreds + romanTens + romanOnes;
     }
     static String Romans(int n, String one, String five, String ten) {
          if (n >= 1) {
              return switch (n) {
                  case 1 -> one;
                  case 2 -> one + one;
                  case 3 -> one + one + one;
                  case 4 -> one + five;
                  case 5 -> five;
                  case 6 -> five + one;
                  case 7 -> five + one + one;
                  case 8 -> five + one + one + one;
                  case 9 -> one + ten;
                  default -> {
                      yield "0";
                  }
              };
          } else {
              return "0";
          }
     }
}
