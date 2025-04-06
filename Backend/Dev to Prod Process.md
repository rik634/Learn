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
- Compile the Code: Use build tools like Maven or Gradle to compile your Java source code into bytecode (.class files).
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
   - File Type: Plain text or console output.
   - Purpose: Logs of the build process that include details about compilation, dependency resolution, and potential errors.
   - Use: Helpful for debugging and monitoring build success.

4. Test Reports (If the tests are included in the build)
   - File Type: HTML, XML, or other formats.
   - Purpose: Summary of test results, including passed, failed, and skipped tests.
   - Use: Ensures code quality and reliability before deployment.

5. Dependency Metadata:
   - Tools like Maven or Gradle generate files to resolve project dependencies:
       - Maven POM.xml: Contains configuration for dependency management and build.
       - Gradle build.gradle: Defines dependencies and build tasks.


6. Distribution Files:
   - Sometimes the build process produces distributable files for deployment. 
   - Example: Pre-packaged zip files or directories containing all necessary files (e.g., libraries, JARs/WARs, configuration files).

7. Docker Image (Optional for containerized applications)
   If containerized:
   - The build process includes creating a Docker image containing the application.
   - This image can be deployed across environments consistently.
    
## Containerization (Optional but powerful)
- It is incorporated to make deployment consistent and scalable.
  Steps:
  - Create a Dockerfile: Here, we create a Dockerfile to package the application. And write the instructions to containerize the application.
    
    ![image](https://github.com/user-attachments/assets/ff63298f-25a2-435a-965c-179e13bba606)
  - Build Docker Image:
    
    ![image](https://github.com/user-attachments/assets/169892aa-233d-4e43-95b7-602ed7475572)
  - Test Locally: Run the Docker container to ensure it works as expected:
    
    ![image](https://github.com/user-attachments/assets/7d7632bb-e266-4c78-8f6d-aa0dfb345172)

##
### Dockerfile:
A Dockerfile is essentially a script containing a set of instructions to build a Docker image. This image packages your Java project along with all its dependencies, configuration, and environment setup into a consistent, portable format. Here's why it's valuable:
- Portability: Your application runs the same way on any system that has Docker installed.
- Consistency: Removes environment-related issues by bundling dependencies and configurations.
- Ease of Deployment: Once an image is built, it can be deployed anywhere, ensuring reliability and scalability.

For example, in a Java project, a typical Dockerfile might:
- Use a base image with Java runtime (e.g., openJDK:17).
- Copy your application code and dependencies into the container.
- Build the application (if needed) and specify the command to run it.
##


## Pre-deployment preparation
This involves preparing the infrastructure and environment for deployment.

## Deployment phase
## Post-deployment phase
## Maintenance and Optimization
