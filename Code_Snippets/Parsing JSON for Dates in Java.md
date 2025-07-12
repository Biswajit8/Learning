https://gemini.google.com/app/3b62876f3d55b58e

```java
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.List;

public class DateTimeValidator {

    private static final DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

    public static boolean validateDateTimeFormats(String jsonString) {
        try {
            ObjectMapper objectMapper = new ObjectMapper();
            JsonNode rootNode = objectMapper.readTree(jsonString);
            List<String> errors = new ArrayList<>();
            validateDateTimeFormatsRecursive(rootNode, errors);
            return errors.isEmpty();
        } catch (Exception e) {
            // Handle exceptions
            e.printStackTrace();
            return false;
        }
    }

    private static void validateDateTimeFormatsRecursive(JsonNode node, List<String> errors) {
        if (node.isContainerNode()) {
            if (node.isObject()) {
                node.fieldNames().forEachRemaining(key -> {
                    if (key.equals("endDateTime") || key.equals("startDateTime")) {
                        String dateTimeString = node.get(key).asText();
                        try {
                            LocalDateTime.parse(dateTimeString, formatter);
                        } catch (Exception e) {
                            errors.add("Invalid date format for " + key + ": " + dateTimeString);
                        }
                    }
                    validateDateTimeFormatsRecursive(node.get(key), errors);
                });
            } else if (node.isArray()) {
                for (JsonNode element : node) {
                    validateDateTimeFormatsRecursive(element, errors);
                }
            }
        }
    }

    public static void main(String[] args) {
        String jsonString = "{\"ruleConfig\":{\"customerRules\":{\"customerPromoWinningRule\":\"EMPLOYEE\",\"empMiliStackableDateList\":[{\"endDateTime\":\"2024-07-10 23:59:00\",\"startDateTime\":\"2024-07-04 01:01:00\"}],\"employeeStacksOnPriceDiscounts\":true,\"militaryStacksOnPriceDiscounts\":false,\"employeeStacksOnOtherPromotions\":[\"ProductLevelPercentDiscount\",\"ProductLevelMultiSKUBIBPercentDiscount\"],\"militaryStacksOnOtherPriceTypes\":[999999],\"militaryStacksOnOtherPromotions\":[\"ProductLevelPercentDiscount\",\"ProductLevelMultiSKUBIBPercentDiscount\"],\"employeeCreditDiscountStackableDateList\":[{\"endDateTime\":\"2020-08-15 23:00:00\",\"startDateTime\":\"2020-08-02 02:00:00\"}],\"itemLevelCouponAndCreditDiscountStackableDateList\":[{\"endDateTime\":\"2020-08-15 23:00:00\",\"startDateTime\":\"2020-08-02 02:00:00\"}]}}}";

        boolean valid = validateDateTimeFormats(jsonString);
        System.out.println("Date formats are valid: " + valid);
    }
}

```

**Explanation**

- The `validateDateTimeFormats` method takes a JSON string as input and returns a boolean indicating format validity.
- It uses a `DateTimeFormatter` to parse date-time strings.
- The `validateDateTimeFormatsRecursive` method recursively traverses the JSON structure.
- If a node with the key `endDateTime` or `startDateTime` is found, it attempts to parse the value using the formatter.
- If parsing fails, an error message is added to the `errors` list.
The main method demonstrates how to use the validation function.

**Improvements**

- For better performance, consider using a dedicated JSON path library or optimizing the recursive traversal.
- For more robust error handling, you can provide detailed error messages or exceptions.
- If you need to handle different time zones, consider using `ZonedDateTime` instead of `LocalDateTime`.

By following these steps and incorporating the provided code, you can effectively validate the date formats in your JSON data.

Get all the Keys in a JSON String Using JsonNode: https://www.baeldung.com/java-jsonnode-get-keys
