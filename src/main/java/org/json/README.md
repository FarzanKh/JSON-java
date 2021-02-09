**Project Milestone 3**
--
**(Added methods in XML.java and test cases in XMLTest.java)**

A new toJSONObject is added to the XML.java which takes a UnaryOperator as a paramter alongside the Reader and and converts the XML to a JSONObject, while transforming all the keys using the UnaryOperator.

To add a prefix to XML keys during parsing XML file, and not in another pass afterwards, we modified the objects passed to the `accumulate` method and took advantage of the Java 8 method reference.

Four new test cases were added that covers every type of XML files and key format such as:

 - An XML file with JSON array
 - An XML file without a JSON array
 - An XML file with CData
 - Long JSON key transformation

Since the user needs to have at least Java 8 to run the Method Reference or Lambda Functions, we changed the `gradle.build` from Java 1.7 to Java 1.8 to make it compatible with new changes.

**How to Run Tests**
- Navigate to the top-level project folder in terminal (should be `/JSON-java`)
- Run the command `gradle clean build test`. Make sure gradle is installed.
- All testing should be done. Testing XML files are included in the top-level of the project and are automatically used by unit tests.
- Note: Testing can also be run in IntelliJ using the exact same methods as used to run tests on the original repository.

**Project Milestone 2**
--
**Added Methods (all in XML.java)**
- Task 1: `static JSONObject toJSONObject(Reader reader, JSONPointer path, boolean hasJSONarray) `
    - Parameters: `reader`: reader with XML file, `path`: key-path to query, `hasJSONarray`: boolean that indicates if the XML file has anything that would result in a JSONArray
        - *note the extra parameter (this was added in order to support JSONArrays by using a different implementation for this case)
    - Returns: `JSONObject` subobject including the last key of the `path` as the key of the subobject
    - How it works: 
        - We added 2 other methods to help (`private static JSONObject toJSONObject(Reader reader, JSONPointer path)` and `private static boolean parse2(XMLTokener x, JSONObject context, String name, XMLParserConfiguration config)`)
        - logic inside this method: uses similar logic to milestone 1 task 2 if hasJSONArray, if !hasJSONArray, calls the logic below:
        - logic inside `toJSONObject(Reader reader, JSONPointer path)`: finds the location of the last key in the path, then parses the relevant JSONObject using `parse2`, which throws a FinishedParsingException once the end of the JSONObject is found.
            - *note: the toJSONObject method that is called for !hasJSONArray assumes a well-formatted XML file and a valid JSONPointer.
 
- Task 2: `static JSONObject toJSONObject(Reader reader, JSONPointer path, JSONObject replacement) `
    - Parameters: `reader`: reader with XML file, `path`: key-path to query, `replacement`: replacement JSONObject
    - Returns: `JSONObject` with subobject replaced.
    - How it works: uses similar logic to milestone 1 task 5, since the method must parse the entire file anyway.  
        - *note: for replacing an index in a JSONArray, the replacement object is placed at the last index

**How to Run Tests**
- Navigate to the top-level project folder in terminal (should be `/JSON-java`)
- Run the command `gradle clean build test`. Make sure gradle is installed.
- All testing should be done. Testing XML files are included in the top-level of the project and are automatically used by unit tests.

**Added Tests (all in XMLTest.java)**  

_Task 1 Tests_  
- Without JSONArray
     - Random middle subobject
     - Last level subobject
- With JSONArray
     - Random middle subobject
     - Last level subobject
     - Invalid pointer, should throw exception
    

_Task 2 Tests_
- Replace middle subobject
- Replace last level subobject
- Invalid pointer, should throw exception
