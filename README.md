# SQL Injection UNION Attack Lab - Determining Number of Columns

This repository documents solving PortSwigger's "SQL injection UNION attack, determining the number of columns returned by the query" lab. The goal was to identify the number of columns in a vulnerable SQL query by performing a UNION attack.

## Lab Details
- **Objective**: Determine the number of columns in the query by injecting UNION SELECT with NULL values until the response includes an additional row.
- **Vulnerability**: SQL injection in the `category` parameter of a product filter.
- **Tool**: Python with `requests` library, Burp Suite for manual verification.

## Solution Steps
1. **Baseline Analysis**:
   - Request: `GET /filter?category=Gifts`
   - Response: Table with 3 columns (name, price, link).
   - Conclusion: Query likely `SELECT name, price, link FROM products WHERE category = ?`.

2. **Initial Attempts**:
   - Injected ` UNION SELECT NULL --`, but table remained empty.
   - Issue: Injection wasn’t breaking the query context.

3. **Final Injection**:
   - Adjusted to `Gifts' UNION SELECT NULL,NULL,NULL --`.
   - Result: Table populated with `<tr><td>NULL</td><td>NULL</td><td>NULL</td></tr>`.
   - Column Count: **3**.

## Script
- `exploit.py`: Automates the UNION attack, checks table content for rows.
- Key Fix: Prepended `Gifts'` to break the original query and append UNION.

## Learnings
- Importance of query context in SQL injection.
- Debugging with response length and table content.
- Manual verification with Burp Suite is crucial when automation falters.

## Files
- `exploit.py`: Final Python script.
- `output.txt`: Sample output showing success at 3 columns.
- `notes.md`: Space for doubts and clarifications.

## Next Steps
- Explore your doubts in `notes.md`—I’ll address them!
- Apply this to the next lab: retrieving data with UNION.
