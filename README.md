# StringCalculator

Instructions
This kata is designed to help you learn test-first coding and refactoring. To that end, try not to read ahead or guess what the next requirements might be. Work incrementally, and complete as many steps as you can in a 30 minute period. Continue trying the kata from scratch, until you can complete it entirely within 30 minutes.



1.	Create a StringCalculator with a method Add(string numbers) that returns an integer.
   
i.	Start with the simplest test case of an empty string, then 1 number, then 2.

ii.	Solve things as simply as possible!

iii.	An empty string should return a sum of 0.

iv.	numbers can include 0, 1, or 2 integers (e.g. "", "1", "1,2").

v.	Add returns the sum of the integers provided in the string numbers.

vi.	Remember to refactor after each test.



3.	Allow the Add method to handle an unknown number of numbers (in the string).

4.	Allow the Add method to handle new lines between numbers (as well as commas):
5.	
i.	Example: "1\n2,3" returns 6.

ii.	Example: "1,\n" is invalid, but no need to test for it. For this kata we are only concerned with testing correct inputs.

7.	Allow the Add method to handle a different delimiter:
i.	To change the delimiter, the beginning of the string should be a separate line formatted like this:
"//[delimiter]\n[numbers]"
ii.	Example: "//;\n1;2" returns 3 (the delimiter is ";").

iii.	This first line is optional; all existing scenarios (using "," or "\n") should work as before.
The below function works for all above test cases:

9.	Calling Add with a negative number will throw an exception "Negatives not allowed: " and then listing all negative numbers that were in the list of numbers.
i.	Example: "-1,2" throws "Negatives not allowed: -1".

ii.	Example: "2,-4,3,-5" throws "Negatives not allowed: -4,-5".

11.	Numbers greater than 1000 should be ignored.
i.	Example: "1001,2" returns 2.

12.	Delimiters can be any length, using this syntax: "//[|||]\n1|||2|||3" returns 6.

13.	Allow multiple delimiters, using this syntax: "//[|][%]\n1|2%3" returns 6.

14.	Handle multiple delimiters of any length.

To Start out we create a main and test class, StringCalculator and TestTheAddMethod. The Add method parses the string and detects numeric characters. It will add together each one it detects. However it can't detect negatives or orders of magnitude above 10.

    public int Add(String numbers){

    int result = 0;

    for(Character elem : numbers.toCharArray()){
        if(Character.isDigit(elem)){
            int numericValue = elem - '0';
            result += numericValue;
        }
    }
    return result;
    }
To pass the remaining test cases some refactoring was needed. Regex can be used to split the string by matching numerics and dashes (as a minus sign) 

String[] numberArray = numbers.split("[^0-9-]+"); //NumberArray contains an array of all numerics detected and delimiters removed.

Using ParseInt to convert the array elements to integers, we perform tests on the various conditions. Error handler needed in the case of negative numbers, and a check to see if the number exceeds 3 digits.

This is the final code:

    public int Add(String numbers){

        String[] numberArray = numbers.split("[^0-9-]+");

            int sum = 0;
            StringBuilder negativeNumbers = new StringBuilder();

            for (String numberStr : numberArray) {
                if (!numberStr.isEmpty()) {
                    int num = Integer.parseInt(numberStr);

                    if (num < 0) {
                        if (!negativeNumbers.isEmpty()) {
                            negativeNumbers.append(", ");
                        }
                        negativeNumbers.append(num);
                    }

                    if (num <= 1000) {
                        sum += num;
                    }
                }
            }


        if(!negativeNumbers.isEmpty()){
            throw new IllegalArgumentException("Negatives not allowed: " + negativeNumbers);

        } return sum;
    }


