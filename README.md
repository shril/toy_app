```Java
public class Palindromes {

    public Boolean isPalindrome(String word) {

        if (word == null || word.strip().isEmpty()) {
            return Boolean.FALSE;
        }
        word = word.replaceAll("[^a-zA-Z]","");
        //word = word.toLowerCase();

        int left_ptr = 0;
        int right_ptr = word.length() - 1;

        while (left_ptr <= right_ptr) {
            if (toLowerCase(word.charAt(left_ptr)) != toLowerCase(word.charAt(right_ptr))) {
                return Boolean.FALSE;
            }
            left_ptr += 1;
            right_ptr -= 1;
        }
        return Boolean.TRUE;
    }

    public List<String> getLongestPalindrome(String input) {

        LinkedHashSet<String> longestPalindromes = new LinkedHashSet<>();
        int max_length = 0;

        for (int i = 0; i < input.length(); i++) {
            if (!Character.isAlphabetic(input.charAt(i))){
                continue;
            }
            for (int j = i ; j < input.length(); j++) {
                if (!Character.isAlphabetic(input.charAt(j))) {
                    continue;
                }
                String substring = input.substring(i,j+1);
                if ( isPalindrome(substring)) {
                    int substring_length = substring.replaceAll("[^a-zA-Z]","").length();
                    if ( substring_length > max_length) {
                       longestPalindromes.clear();
                       longestPalindromes.add(substring);
                       max_length = substring_length;
                    }
                    else if ( substring_length == max_length) {
                       longestPalindromes.add(substring);
                    }
                }
            }
        }
        return new ArrayList<>(longestPalindromes);
    }

    private void assertIsPalindrome(String input, boolean expected) {
        boolean result = isPalindrome(input);
        assertEquals(result, expected);
    }

    private void assertGetLongestPalindrome(String input, List<String> expected ) {
        List<String> result = getLongestPalindrome(input);
        assertEquals(result, expected);
    }

    @Test
    public void testIsPalindrome() {
        assertIsPalindrome("", false);
        assertIsPalindrome("lotion", false);
        assertIsPalindrome("racecar", true);

        assertIsPalindrome("RaceCar", true);
        assertIsPalindrome("    ", false);
        assertIsPalindrome("Step on no pets", true);
        assertIsPalindrome("Step    onno pets", true);
        assertIsPalindrome("No lemon, no melons", false);

        assertIsPalindrome("Eva,  can I See bees in    a Cave??????", true);
        assertIsPalindrome("A man, a plan, a canal : PAnama!!!12345", true);

    }

    @Test
    public void testGetLongestPalindrome() {
        assertGetLongestPalindrome("Aaaaa", Arrays.asList("Aaaaa"));
        assertGetLongestPalindrome("A racecar", Arrays.asList("racecar"));
        assertGetLongestPalindrome("No lemon, no melons",Arrays.asList("No lemon, no melon"));
        assertGetLongestPalindrome("A racecar reviver", Arrays.asList("racecar","reviver"));

        assertGetLongestPalindrome("", new ArrayList<>());
        assertGetLongestPalindrome("Racecars racing", Arrays.asList("Racecar", "cars rac"));
        assertGetLongestPalindrome("Ra_ce% ca rs racing",Arrays.asList("Ra_ce% ca r","ca rs rac"));
        assertGetLongestPalindrome("Abcab",Arrays.asList("A","b","c","a"));
        assertGetLongestPalindrome("AbcaB",Arrays.asList("A","b","c","a","B"));
    }
}

```
