# Build stage
FROM maven:3.6.3-openjdk-17-slim AS build

WORKDIR /build
COPY backend/pom.xml .
RUN mvn dependency:go-offline
COPY backend/src ./src
RUN mvn clean package -DskipTests


#Runtime stage
FROM amazoncorretto:17

WORKDIR /app
COPY --from=build /build/target/Internship-*.jar /app/
#zedt line eli taht
COPY wait-for-it.sh /app/ 
EXPOSE 8085
ENV DB_URL=jdbc:mysql://internship_management_db:3306/Internship-Management-System
CMD java -jar -Dspring.datasource.url=${DB_URL} Internship-Management-System-2.1.1.jar
#*ENTRYPOINT ["java", "-jar", "Internship-Management-System-2.1.1.jar"]
ENTRYPOINT ["./wait-for-it.sh", "internship_management_db:3306", "--", "java", "-jar", "-Dspring.datasource.url=${DB_URL}", "Internship-Management-System-2.1.1.jar"]
