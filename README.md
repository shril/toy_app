```Java
public class Main {
    public static void main(String[] args) {
        List<List<String>> badge_records = Arrays.asList(
                Arrays.asList("Martha","exit"),
                Arrays.asList("Paul","enter"),
                Arrays.asList("Martha","enter"),
                Arrays.asList("Martha","exit"),
                Arrays.asList("Jennifer","enter"),
                Arrays.asList("Paul","enter"),
                Arrays.asList("Curtis","enter"),
                Arrays.asList("Paul","exit"),
                Arrays.asList("Martha","enter"),
                Arrays.asList("Martha","exit"),
                Arrays.asList("Jennifer","exit")
        );
        System.out.println(mismatched(badge_records));
    }

    public static List<List<String>> mismatched(List<List<String>> records) {
        Map<String, Integer> peopleActivityCount = new HashMap<>(); // { 'name': count }
        Set<String> entryMismatched = new HashSet<>(), exitMismatched = new HashSet<>();
        for (List<String> record: records) {
            String name = record.get(0);
            String activity = record.get(1);

            peopleActivityCount.putIfAbsent(name, 0);
            if ("enter".equals(activity)) {
                peopleActivityCount.put(name, peopleActivityCount.get(name) + 1);
            } else {
                peopleActivityCount.put(name, peopleActivityCount.get(name) - 1);
            }
            System.out.println(name + " " + peopleActivityCount.get(name));
            int activityCount = peopleActivityCount.get(name);
            if (activityCount == -1) {
                System.out.println("Adding " + name + " to exit list.");
                exitMismatched.add(name);
                peopleActivityCount.put(name, 0);
            }
            if (activityCount == 2) {
                System.out.println("Adding " + name + " to entry list.");
                entryMismatched.add(name);
                peopleActivityCount.put(name, 1);
            }
        }
        // Now that records are complete, check whose number is still imbalanced
        for (Map.Entry<String, Integer> record: peopleActivityCount.entrySet()) {
            if (record.getValue().equals(1)) {
                entryMismatched.add(record.getKey());
            }
        }
        List<List<String>> result = new ArrayList<>();
        result.add(entryMismatched.stream().toList());
        result.add(exitMismatched.stream().toList());
        return result;
    }

}
```
