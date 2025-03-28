# Process

## Development phase

This phase involves developing the application.
Steps:
- Write the Code: Develop your application (e.g., using Java, Spring Boot) with clear functionality, clean architecture, and secure coding practices.
- Version Control: Use Git to track changes and collaborate effectively. Commit changes frequently.
- Local Testing: Test your code for functionality using JUnit or other testing frameworks.
  
## Build phase
This step involves compiling the project and preparing it for deployment.
Steps:
- Compile the Code: Use build tools like Maven or Gradle to compile your Java source code into bytecode ( files).
- Resolve Dependencies: The build tools handle external libraries (e.g., Spring Boot dependencies).
- Package the Application:
  - JAR: Package the compiled code and resources into a .jar file for standalone applications.
  - WAR: Package it as a .war file for web applications.
- Run Tests: Automated tests (unit, integration) are executed during the build process. These tests are run to ensure code quality and functionality.
- Build outputs: The final deliverable (artifact) is generated, which can be deployed.

These build output files often include the following:
1. Compiled Bytecode:
   - File Type: .class files
   - Purpose: These files are the result of compiling the java source code (.java files). They contain bytecode that can be interpreted by the Java Virtual Machine (JVM).
   - Location: Typically stored in "/bin" or "/target/classes" directory in project structure.

2. Packaged Artifacts:
   - These are the bundled forms of application, containing the compiled bytecode, resources (e.g., configuration files, images), and external dependencies.
   
      JAR (Java Archive):
      - A single file that includes the compiled code and resources.
      - Used for applications, libraries, or reusable modules.
      - Example: myApp.jar
      - It can be run with "java -jar myApp.jar".
      
      WAR (Web Application Archive):
      - Specifically for web applications using java (e.g., with servlet or JSP).
      - Used in web servers like Apache Tomcat.
      - Example: myWebApp.war

      EAR (Enterprise Application Archive)
      - For enterprise applications deployed on Java EE servers (e.g., JBoss, WebLogic)
      - Contains multiple modules (like JARs and WARs).

3. Build Logs:


4. Test Reports (If the tests are included in the build)


5. Dependency Metadata:


6. Distribution Files:

7. Docker Image (Optional for containerized applications)

## Containerization (Optional but powerful)
- 
## Pre-deployment preparation
## Deployment phase
## Post-deployment phase
## Maintenance and Optimization
