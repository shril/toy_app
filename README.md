```Java
public class LargeNumberAddition {

    public String add(String number1, String number2) {
        StringBuilder result = new StringBuilder();
        int i, j, carry = 0;

        for (i = number1.length() - 1, j = number2.length() - 1; i >= 0 && j >= 0; i--, j--) {
            int sum = Character.getNumericValue(number1.charAt(i)) + Character.getNumericValue(number2.charAt(j)) + carry;
            result.insert(0, sum%10);
            carry = sum / 10;
        }
        while (i >= 0) {
            int sum = Character.getNumericValue(number1.charAt(i)) + carry;
            result.insert(0, sum%10);
            carry = sum / 10;
            i -= 1;
        }
        while (j >= 0) {
            int sum = Character.getNumericValue(number2.charAt(j)) + carry;
            result.insert(0, sum%10);
            carry = sum / 10;
            j -= 1;
        }
        if (carry > 0) {
            result.insert(0, 1);
        }
        return result.toString();
    }

    public String addWithCommas(String number1, String number2) {
        number1 = number1.replace(",","");
        number2 = number2.replace(",","");

        String result = add(number1, number2);

        return addCommas(result);
    }

    private String addCommas(String number) {
        StringBuilder num = new StringBuilder(number);
        for (int i = number.length()-3 ; i > 0; i-=3 ) {
            num.insert(i, ",");
        }
        return num.toString();
    }

    public String fibonacci(int n) {
        if (n <= 2)
            return "1";
        List<String> sequence = new ArrayList<>();
        sequence.add("1");
        sequence.add("1");
        for ( int i = 2; i < n; i++) {
            sequence.add(add(sequence.get(i-1),sequence.get(i-2)));
        }
        return sequence.getLast();
    }
}

public class LargeNumberAddition_Test {

    LargeNumberAddition lna;

    public LargeNumberAddition_Test() {
        lna = new LargeNumberAddition();
    }

    @Test
    public void testAdd() {
        assertAdd("99999","1","100000");
        assertAdd("1","8","9");
        assertAdd("15","15","30");
        assertAdd("3456","1234","4690");
        // Large Numbers
        assertAdd("9999999999999999","11111111111111111111","11121111111111111110");

    }

    @Test
    public void testAddWithCommas() {
        assertAddWithCommas("99,999","1","100,000");
        assertAddWithCommas("1","8","9");
        assertAddWithCommas("15","15","30");
        assertAddWithCommas("3,456","1,234","4,690");
        //Commas test
        assertAddWithCommas("123,456","111,234","234,690");
        assertAddWithCommas("123,456","999,234","1,122,690");
        assertAddWithCommas("1,123,456","111,234","1,234,690");
        // Large Numbers
        assertAddWithCommas("9,999,999,999,999,999","11,111,111,111,111,111,111","11,121,111,111,111,111,110");

    }

    public void assertAdd(String a, String b, String expected) {
        String answer = lna.add(a,b);
        assertEquals(answer, expected);
    }

    public void assertAddWithCommas(String a, String b, String expected) {
        String answer = lna.addWithCommas(a,b);
        assertEquals(answer, expected);
    }
}

```
