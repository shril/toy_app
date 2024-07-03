```Java
public class CategoryCollection {
    Map<String, String> categoryMap;
    public CategoryCollection() {
        categoryMap = new HashMap<>();
    }
    public void addCategory(String childCategory, String parentCategory) {
        categoryMap.put(childCategory, parentCategory);
    }
    public Map<String, String> getCategoryMap() {
        return categoryMap;
    }
}

public class Coupon {

    private final String couponName;
    private final String dateModified;
    public Coupon( String couponName, String dateModified) {
        this.couponName = couponName;
        this.dateModified = dateModified;
    }
    public String getCouponName() {
        return couponName;
    }
    public String getDateModified() {
        return dateModified;
    }
}

public class CouponCollection {

    Map<String, List<Coupon>> couponMap;
    Map<String, String> categoryMap;
    String todaysDate;

    public CouponCollection( CategoryCollection categories ) {
        couponMap = new HashMap<>();
        categoryMap = categories.getCategoryMap();
        LocalDate today = LocalDate.now();
        todaysDate = today.toString();
    }

    public void addCoupon(String coupon, String dateModified, String category) {
        Coupon c = new Coupon(coupon, dateModified);
        couponMap.putIfAbsent(category, new ArrayList<>());
        couponMap.get(category).add(c);
        // Sort in reverse based on dates
        try {
            couponMap.get(category).sort(new Comparator<Coupon>() {
                @Override
                public int compare(Coupon c1, Coupon c2) {
                    return c2.getDateModified().compareTo(c1.getDateModified());
                }
            });
        } catch (Exception ex) {
            throw new RuntimeException("Error creating coupon collection.");
        }
    }

    public String findDatedCouponForCategory(String category) {
        if ( couponMap.containsKey(category)) {
            int couponIdx = 0;
            while (couponMap.get(category).get(couponIdx).getDateModified().compareTo(todaysDate) > 0 ) {
                couponIdx += 1;
            }
            return couponMap.get(category).get(couponIdx).getCouponName();
        } else if ( categoryMap.containsKey(category)) {
            return findCouponForCategory(categoryMap.get(category));
        }
        return "No Coupon";
    }

    public String findCouponForCategory(String category) {
        if ( couponMap.containsKey(category)) {
            return couponMap.get(category).get(0).getCouponName();
        } else if ( categoryMap.containsKey(category)) {
            return findCouponForCategory(categoryMap.get(category));
        }
        return "No Coupon";
    }
}

public class Main {
    public static void main(String[] args) {

        CategoryCollection categories = new CategoryCollection();
        categories.addCategory("Comforter Sets", "Bedding");

        CouponCollection coupons = new CouponCollection(categories);
        coupons.addCoupon("Comforter Sale", "2024-01-01", "Comforter Sets");

        // Positive Test Cases
        assert Objects.equals(coupons.findCouponForCategory("Comforter Sets"), "Comforter Sale") : "Failed Test 1";
        assert Objects.equals(coupons.findCouponForCategory("Bedding"), "Bedding Bargains") : "Failed Test 2";
      
        // Negative Test Cases
        assert Objects.equals(coupons.findCouponForCategory("Unknown Category"), "No Coupon") : "Failed Test 8";
    }
}
```
