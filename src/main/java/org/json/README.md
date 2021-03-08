## Project Milestone 5

#### (One new method and one new class is added to XML.java)

For this milestone, one new methods is added that asynchronously converts an XML file to a JSONObject, then runs a user-defined function in the case of either success or error. 

### The method

The method takes in 3 parameters: 
1) `Reader reader`: a reader containing the XML file
2) `Consumer<JSONObject> consumer`: a consumer function to run in the case of success.
3) `Consumer<Exception> exception`: a consumer function to run in the case of error.

The method makes use of a `Callable` class that is used by `ExecutorService` to return a future. After the completion or error of running the callable, the corresponding function is run.

### Tests (XMLTest.java)
1) `TestXMLConcurrent`: Given an XML file, test whether successfully converted to JSONObject and assert that it is equal to expected JSONObject.
2) `TestXMLConcurrentError`: Given an invalid XML file (`exception.xml`), assert that exception is properly triggered.

## Project Milestone 4

#### (One new method is added to JSONObject.java and two new methods are added to XML.java. Moreover, Test cases are in XMLTest.java)

For this milestone, three new methods are added that each expose some form of streaming to the application code.
One of these methods are added under the _JSONObject_ class, and the other two are added under the _XML_ class.

### Main Streaming Functionality
A new class with the name `simpleJSONNode` was created that acts similar to a Map.Entry, with one key and one value. Using an iterator of this new class, the method `toStream` was implemented in the JSONObject class to expose custom streaming functionality
to the library. For example, the user can take advantage of the `toStream` method to obtain the keys and values of a the top level of the XML file.  
Several JUnit test cases were written in `JSONObjectTest` and `XMLTest` (found at the end of each file) to ensure new functionalities are reliable and robust.


### Extra Streaming Functionalities
The method `readFileStream_wordStart` which is added in the XML class reads all lines of the XML file line by line and finds words that start with a given filter.
For example, calling `XML.readFileStream_wordStart(filePath, "b")` streams the whole XML file line by line and finds those words that start with the letter 'b'.

Furthermore, we added `readFileStream_wordEnd` which works similar to `readFileStream_wordStart`, but it will find words that end with a given letter.

**How to Run Tests**
- Navigate to the top-level project folder in terminal (should be `/JSON-java`)
- Run the command `gradle clean build test`. Make sure gradle is installed.
- All testing should be done. Testing XML files are included in the top-level of the project and are automatically used by unit tests.
- Note: The tests were created on a Mac, so there may be errors with line separators with the tests if running in an IDE. If so, make sure line separation is set to LF (not CRLF).


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
