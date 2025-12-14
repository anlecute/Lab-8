# Task 5.1: Search Customers - Implementation Report

## Requirements

- Add search functionality to find customers by keyword (fullName, email, or customerCode).
- Update `CustomerRepository` with a custom query method.
- Update Service layer to support search and return DTOs.
- Add a new endpoint in Controller for searching customers.

## Implementation

1. **CustomerRepository**
   - Added method with @Query annotation:
     ```java
     @Query("SELECT c FROM Customer c WHERE " +
            "LOWER(c.fullName) LIKE LOWER(CONCAT('%', :keyword, '%')) OR " +
            "LOWER(c.email) LIKE LOWER(CONCAT('%', :keyword, '%')) OR " +
            "LOWER(c.customerCode) LIKE LOWER(CONCAT('%', :keyword, '%'))")
     List<Customer> searchCustomers(@Param("keyword") String keyword);
     ```
2. **Service Layer**
   - Added method `List<CustomerResponseDTO> searchCustomers(String keyword);` in interface and implementation.
   - Converts found entities to DTOs.
3. **Controller**
   - Added endpoint:
     ```java
     @GetMapping("/search")
     public ResponseEntity<List<CustomerResponseDTO>> searchCustomers(@RequestParam String keyword) {
         List<CustomerResponseDTO> customers = customerService.searchCustomers(keyword);
         return ResponseEntity.ok(customers);
     }
     ```

## Test

- Example request:
  ```
  curl "http://localhost:8080/api/customers/search?keyword=john"
  ```
- Returns a list of customers matching the keyword in name, email, or code.

## Status

- All requirements of Task 5.1 are fully implemented and tested.
