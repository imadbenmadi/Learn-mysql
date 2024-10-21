Got it! Let's assume we have this data:

### **Employees** Table
| employee_id | name          | department_id | salary  |
|-------------|---------------|---------------|---------|
| 101         | John Doe      | 1             | 60000   |
| 102         | Jane Smith    | 2             | 80000   |
| 103         | Mike Johnson  | 2             | 75000   |
| 104         | NULL          | 3             | NULL    |

### **Departments** Table
| department_id | department_name |
|---------------|-----------------|
| 1             | HR              |
| 2             | Engineering     |
| 3             | Marketing       |
| 4             | Sales           |

### **What happens with different JOIN queries?**

#### 1. **INNER JOIN**
- Returns only rows that have matching values in both tables.
  
```sql
SELECT e.name, d.department_name
FROM Employees e
INNER JOIN Departments d ON e.department_id = d.department_id;
```

**Result:**
| name          | department_name |
|---------------|-----------------|
| John Doe      | HR              |
| Jane Smith    | Engineering     |
| Mike Johnson  | Engineering     |

- Explanation: It only returns employees who have a matching `department_id` in the `Departments` table. The `NULL` in the `name` field or non-matching department (like `Sales`) is excluded.

#### 2. **LEFT JOIN**
- Returns all rows from the left table (`Employees`) and matched rows from the right (`Departments`). Unmatched departments will be `NULL`.

```sql
SELECT e.name, d.department_name
FROM Employees e
LEFT JOIN Departments d ON e.department_id = d.department_id;
```

**Result:**
| name          | department_name |
|---------------|-----------------|
| John Doe      | HR              |
| Jane Smith    | Engineering     |
| Mike Johnson  | Engineering     |
| NULL          | Marketing       |

- Explanation: All employees are included, even if their department doesn't exist (e.g., no department for "NULL" employee). The `Marketing` employee has no corresponding name in `Employees`, but the department is shown.

#### 3. **RIGHT JOIN**
- Returns all rows from the right table (`Departments`) and matched rows from the left (`Employees`). Unmatched employees will have `NULL` in their place.

```sql
SELECT e.name, d.department_name
FROM Employees e
RIGHT JOIN Departments d ON e.department_id = d.department_id;
```

**Result:**
| name          | department_name |
|---------------|-----------------|
| John Doe      | HR              |
| Jane Smith    | Engineering     |
| Mike Johnson  | Engineering     |
| NULL          | Marketing       |
| NULL          | Sales           |

- Explanation: All departments are shown, even if there are no employees in that department. For example, `Sales` appears with no employee, and `Marketing` shows `NULL` for the employee name.

#### 4. **FULL JOIN (emulated using LEFT JOIN + UNION RIGHT JOIN)**
- Returns all rows where there is a match in either `Employees` or `Departments`.

```sql
SELECT e.name, d.department_name
FROM Employees e
LEFT JOIN Departments d ON e.department_id = d.department_id
UNION
SELECT e.name, d.department_name
FROM Employees e
RIGHT JOIN Departments d ON e.department_id = d.department_id;
```

**Result:**
| name          | department_name |
|---------------|-----------------|
| John Doe      | HR              |
| Jane Smith    | Engineering     |
| Mike Johnson  | Engineering     |
| NULL          | Marketing       |
| NULL          | Sales           |

- Explanation: This combines the results of the `LEFT JOIN` and `RIGHT JOIN`, showing all employees and all departments. Unmatched rows from either table are included.

### Summary:
- **INNER JOIN**: Only shows rows with matching `department_id` in both tables (excludes unmatched rows).
- **LEFT JOIN**: Shows all employees, even if there's no matching department (`NULL` in `department_name`).
- **RIGHT JOIN**: Shows all departments, even if there's no matching employee (`NULL` in `name`).
- **FULL JOIN** (emulated): Shows everything from both tables, filling in `NULL` for missing matches.

