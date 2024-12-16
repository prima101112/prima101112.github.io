---
title: "10 Important Prompt for Software Engineers"
date: 2024-12-16T13:25:09+07:00
draft: false
---

### 1. **Code Refactoring**  
- **Prompt**: *"Refactor this code to make it cleaner and more Pythonic: `{code}`"*  
- **Prompt with Example**: *"Refactor this code: ```for i in range(len(arr)): print(arr[i])```"*  
- **What for**: To improve code readability, performance, or adhere to best practices.  
- **Example**:  
    ```python  
    for item in arr:  
        print(item)  
    ```

---

### 2. **Bug Fixing**  
- **Prompt**: *"Fix the bug in this code: `{code}` and explain what went wrong."*  
- **Prompt with Example**: *"Fix the bug in this code: ```x = int('123a')```"*  
- **What for**: To debug and resolve issues in the code.  
- **Example**:  
    ```python  
    try:  
        x = int('123a')  
    except ValueError:  
        print("Invalid input, cannot convert to integer.")  
    ```

---

### 3. **Code Generation**  
- **Prompt**: *"Write a function to `{task}` in `{language}`."*  
- **Prompt with Example**: *"Write a function to reverse a string in Python."*  
- **What for**: To quickly generate functions or reusable code snippets.  
- **Example**:  
    ```python  
    def reverse_string(s):  
        return s[::-1]  

    print(reverse_string("hello"))  # Output: "olleh"  
    ```

---

### 4. **Write Unit Tests**  
- **Prompt**: *"Write unit tests for the following function: `{code}`."*  
- **Prompt with Example**: *"Write unit tests for this function: ```def add(x, y): return x + y```"*  
- **What for**: To ensure code behaves as expected through automated tests.  
- **Example**:  
    ```python  
    import unittest  

    def add(x, y):  
        return x + y  

    class TestAddFunction(unittest.TestCase):  
        def test_add(self):  
            self.assertEqual(add(2, 3), 5)  
            self.assertEqual(add(-1, 1), 0)  

    unittest.main()  
    ```

---

### 5. **SQL Query Generation**  
- **Prompt**: *"Write an SQL query to `{task}`."*  
- **Prompt with Example**: *"Write an SQL query to find all users older than 30."*  
- **What for**: To create or optimize SQL queries for databases.  
- **Example**:  
    ```sql  
    SELECT *  
    FROM users  
    WHERE age > 30;  
    ```

---

### 6. **Explain Code**  
- **Prompt**: *"Explain what the following code does: `{code}`."*  
- **Prompt with Example**: *"Explain this code: ```for i in range(3): print(i)```"*  
- **What for**: To clarify or document code for better understanding.  
- **Example**:  
    *"The loop iterates over values `0`, `1`, and `2`, printing each value."*

---

### 7. **Code Conversion**  
- **Prompt**: *"Convert this `{source_language}` code to `{target_language}`: `{code}`."*  
- **Prompt with Example**: *"Convert this Python code to JavaScript: ```print('Hello')```"*  
- **What for**: To translate code between programming languages.  
- **Example**:  
    ```javascript  
    console.log('Hello');  
    ```

---

### 8. **Regex Creation**  
- **Prompt**: *"Write a regex to `{task}`."*  
- **Prompt with Example**: *"Write a regex to validate email addresses."*  
- **What for**: To validate or extract patterns from strings using regular expressions.  
- **Example**:  
    ```regex  
    ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$  
    ```

---

### 9. **Generate API Request Code**  
- **Prompt**: *"Write code to send a `{request_type}` request to `{url}` using `{language}`."*  
- **Prompt with Example**: *"Write code to send a GET request to `https://api.example.com/data` using Python."*  
- **What for**: To create API integration code.  
- **Example**:  
    ```python  
    import requests  

    response = requests.get("https://api.example.com/data")  
    print(response.json())  
    ```

---

### 10. **System Design Suggestions**  
- **Prompt**: *"Design a system for `{use_case}` and describe its architecture."*  
- **Prompt with Example**: *"Design a system for an image upload service."*  
- **What for**: To outline a scalable system design or architecture.  
- **Example**:  
    *"Use the following components:*  
    1. **Storage**: AWS S3 for image storage.  
    2. **API**: Load-balanced servers to handle upload requests.  
    3. **Processing**: Background worker to resize or process images.  
    4. **Delivery**: Use a CDN for fast delivery of images."*
    

To be honnest this is created by AI, with **my great prompt**. prompting is the key of learning anything now. when it used to be a **google search technique**. 
If content in Internet now somehow could easyly created by AI **is the prompt is actually more important to kmow than the content it self?**
just wondering.

but if a prompt could create also by AI so whats is the original thinking and ideas of a writer is it 

`create me a prompt to actually writing a full script of a really interesting fictional stories` ?

`create me a prompt to actually build full flaged API golang application (per files and folder structure) including the database migration. the application is a Simple POS system` ?

I AM WONDERING.

