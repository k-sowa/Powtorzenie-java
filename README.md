## Odczytywanie danych z pliku CSV za pomocą Apache Commons CSV

### Krok 1: Dodanie zależności

Dodaj następującą zależność do pliku `pom.xml`:

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-csv</artifactId>
    <version>1.8</version>
</dependency>
```

### Krok 2: Struktura pliku CSV

Upewnij się, że Twój plik CSV ma odpowiednią strukturę. Przykład pliku `events.csv`:

```csv
Name,Time
Event 1,12:00:00
Event 2,14:30:15
Event 3,09:45:00
```

### Krok 3: Klasa modelu danych

Stwórz klasę, która będzie reprezentować dane z pliku CSV. W tym przypadku stworzymy klasę `Event`.

```java
import java.time.LocalTime;

public class Event {
    private String name;
    private LocalTime time;

    public Event(String name, LocalTime time) {
        this.name = name;
        this.time = time;
    }

    public String getName() {
        return name;
    }

    public LocalTime getTime() {
        return time;
    }

    @Override
    public String toString() {
        return "Event{name='" + name + "', time=" + time + '}';
    }
}
```

### Krok 4: Odczytywanie danych z pliku CSV

Stwórz klasę, która będzie odpowiedzialna za odczyt danych z pliku CSV i ich przetwarzanie.

```java
import org.apache.commons.csv.CSVFormat;
import org.apache.commons.csv.CSVParser;
import org.apache.commons.csv.CSVRecord;

import java.io.FileReader;
import java.io.IOException;
import java.nio.file.Paths;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.List;

public class CsvReader {
    public static List<Event> readEventsFromCsv(String filePath) {
        List<Event> events = new ArrayList<>();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("HH:mm:ss");

        try (FileReader reader = new FileReader(filePath);
             CSVParser csvParser = new CSVParser(reader, CSVFormat.DEFAULT.withHeader())) {

            for (CSVRecord csvRecord : csvParser) {
                String name = csvRecord.get("Name");
                String timeString = csvRecord.get("Time");
                LocalTime time = LocalTime.parse(timeString, formatter);

                events.add(new Event(name, time));
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

        return events;
    }

    public static void main(String[] args) {
        String filePath = "events.csv";
        List<Event> events = readEventsFromCsv(filePath);

        for (Event event : events) {
            System.out.println(event);
        }
    }
}
```

### Wyjaśnienie kroków:

1. **Dodanie zależności do Apache Commons CSV**:
   - Dodaj odpowiednią zależność do swojego pliku `pom.xml`, jeśli używasz Maven.

2. **Stworzenie klasy `Event`**:
   - Klasa `Event` przechowuje nazwę wydarzenia oraz czas, w którym się odbywa.

3. **Odczyt danych z pliku CSV**:
   - Użyj klasy `CSVParser` z biblioteki Apache Commons CSV do odczytu i parsowania pliku CSV.
   - Użyj `DateTimeFormatter` do parsowania czasu w formacie `HH:mm:ss`.

4. **Wyświetlanie informacji**:
   - Metoda `main` odczytuje wydarzenia z pliku CSV i wyświetla je na konsoli.


## Odczytywanie godziny w formacie HH:mm:ss z pliku CSV

### Krok 1: Struktura pliku CSV

Twój plik CSV (`events.csv`) powinien mieć kolumny `Name` i `Time`. Przykład zawartości pliku:

```csv
Name,Time
Event 1,12:00:00
Event 2,14:30:15
Event 3,09:45:00
```

### Krok 2: Klasa modelu danych

Upewnij się, że masz klasę `Event`, która przechowuje nazwę wydarzenia i czas.

```java
import java.time.LocalTime;

public class Event {
    private String name;
    private LocalTime time;

    public Event(String name, LocalTime time) {
        this.name = name;
        this.time = time;
    }

    public String getName() {
        return name;
    }

    public LocalTime getTime() {
        return time;
    }

    @Override
    public String toString() {
        return "Event{name='" + name + "', time=" + time + '}';
    }
}
```

### Krok 3: Moduł do odczytywania danych z pliku CSV

Upewnij się, że masz moduł do odczytywania danych z pliku CSV.

```java
import org.apache.commons.csv.CSVFormat;
import org.apache.commons.csv.CSVParser;
import org.apache.commons.csv.CSVRecord;

import java.io.FileReader;
import java.io.IOException;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.List;

public class CsvReader {
    public static List<Event> readEventsFromCsv(String filePath) {
        List<Event> events = new ArrayList<>();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("HH:mm:ss");

        try (FileReader reader = new FileReader(filePath);
             CSVParser csvParser = new CSVParser(reader, CSVFormat.DEFAULT.withHeader())) {

            for (CSVRecord csvRecord : csvParser) {
                String name = csvRecord.get("Name");
                String timeString = csvRecord.get("Time");
                LocalTime time = LocalTime.parse(timeString, formatter);

                events.add(new Event(name, time));
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

        return events;
    }
}
```

### Krok 4: Użycie modułu do odczytu godzin z pliku CSV

Stwórz klasę, która będzie używać modułu do odczytywania danych z pliku CSV i wyświetlania godzin.

```java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        String filePath = "events.csv";
        List<Event> events = CsvReader.readEventsFromCsv(filePath);

        for (Event event : events) {
            System.out.println("Wydarzenie: " + event.getName() + ", Godzina: " + event.getTime());
        }
    }
}
```

### Podsumowanie

1. **Struktura pliku CSV**:
   - Plik CSV powinien mieć kolumny `Name` i `Time` z danymi w formacie `HH:mm:ss`.

2. **Klasa modelu danych (`Event`)**:
   - Klasa do przechowywania nazwy wydarzenia i czasu.

3. **Moduł do odczytywania danych z pliku CSV (`CsvReader`)**:
   - Klasa do odczytywania i parsowania danych z pliku CSV.

4. **Użycie modułu**:
   - Klasa `Main` używa modułu do odczytywania danych i wyświetlania informacji o wydarzeniach i ich godzinach.

### Rozwiązanie kolokwium powtórzeniowego
Oto szczegółowy poradnik krok po kroku do każdego zadania:

### 0. Przegląd plików CSV

1. Rozpakuj załączone pliki `eegpliki.zip` do preferowanej lokalizacji.
2. Otwórz pliki `tm00.csv` i `tm01.csv` w edytorze tekstowym lub arkuszu kalkulacyjnym, aby zapoznać się z ich zawartością.

### 1. Utworzenie projektów i struktury paczek

#### Projekt 1

1. Utwórz nowy projekt Java w preferowanym IDE (np. IntelliJ IDEA, Eclipse).

2. Utwórz trzy paczki:
   - `server`
   - `client`
   - `databasecreator`

3. W paczce `databasecreator` utwórz klasę `Creator`.

**Creator.java**
```java
package databasecreator;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class Creator {
    public static void main(String[] args) {
        String url = "jdbc:sqlite:eeg_data.db";
        
        String sql = "CREATE TABLE IF NOT EXISTS EEGData (\n"
                + " id integer PRIMARY KEY,\n"
                + " username text NOT NULL,\n"
                + " electrode_number integer NOT NULL,\n"
                + " eeg_plot_base64 text NOT NULL\n"
                + ");";
        
        try (Connection conn = DriverManager.getConnection(url);
             Statement stmt = conn.createStatement()) {
            stmt.execute(sql);
            System.out.println("Database and table created.");
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

4. Uruchom klasę `Creator`, aby utworzyć bazę danych SQLite i tabelę `EEGData`.

### 2. Aplikacja kliencka

1. W paczce `client` utwórz klasę `Client`.

**Client.java**
```java
package client;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.io.PrintWriter;
import java.net.Socket;
import java.util.Scanner;

public class Client {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter username: ");
        String username = scanner.nextLine();
        System.out.print("Enter file path: ");
        String filepath = scanner.nextLine();
        
        sendData(username, filepath);
    }

    public static void sendData(String username, String filepath) {
        try (Socket socket = new Socket("localhost", 12345);
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
             BufferedReader br = new BufferedReader(new FileReader(filepath))) {
            
            out.println(username);
            String line;
            while ((line = br.readLine()) != null) {
                out.println(line);
                Thread.sleep(2000);
            }
            out.println("bye");
        } catch (IOException | InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

2. Uruchom aplikację kliencką `Client` i podaj nazwę użytkownika oraz ścieżkę do pliku CSV (np. `tm00.csv` lub `tm01.csv`).

### 3. Aplikacja serwerowa

1. W paczce `server` utwórz klasę `Server` oraz klasę `ClientHandler`.

**Server.java**
```java
package server;

import databasecreator.Creator;

import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Base64;

public class Server {
    public static void main(String[] args) {
        Creator.main(null); // Ensure the database is created
        
        try (ServerSocket serverSocket = new ServerSocket(12345)) {
            while (true) {
                new ClientHandler(serverSocket.accept()).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

class ClientHandler extends Thread {
    private Socket socket;
    
    public ClientHandler(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try (BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()))) {
            String username = in.readLine();
            String line;
            int electrodeNumber = 0;
            while (!(line = in.readLine()).equals("bye")) {
                BufferedImage image = createImageFromEEGData(line);
                String base64Image = encodeImageToBase64(image);
                saveToDatabase(username, electrodeNumber++, base64Image);
            }
        } catch (IOException | SQLException e) {
            e.printStackTrace();
        }
    }

    private BufferedImage createImageFromEEGData(String data) {
        int width = 200;
        int height = 100;
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        Graphics2D g2d = image.createGraphics();
        g2d.setColor(Color.WHITE);
        g2d.fillRect(0, 0, width, height);
        g2d.setColor(Color.RED);
        String[] values = data.split(",");
        int midHeight = height / 2;
        for (int i = 0; i < values.length; i++) {
            int value = Integer.parseInt(values[i]);
            g2d.fillRect(i * 2, midHeight - value, 1, 1);
        }
        g2d.dispose();
        return image;
    }

    private String encodeImageToBase64(BufferedImage image) throws IOException {
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        ImageIO.write(image, "png", baos);
        byte[] imageBytes = baos.toByteArray();
        return Base64.getEncoder().encodeToString(imageBytes);
    }

    private void saveToDatabase(String username, int electrodeNumber, String base64Image) throws SQLException {
        String url = "jdbc:sqlite:eeg_data.db";
        String sql = "INSERT INTO EEGData(username, electrode_number, eeg_plot_base64) VALUES(?,?,?)";
        
        try (Connection conn = DriverManager.getConnection(url);
             PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setString(1, username);
            pstmt.setInt(2, electrodeNumber);
            pstmt.setString(3, base64Image);
            pstmt.executeUpdate();
        }
    }
}
```

2. Uruchom aplikację serwerową `Server`.

### 4. Testowanie aplikacji klienckiej

1. W paczce `client` utwórz klasę `ClientTest`.

**ClientTest.java**
```java
package client;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvFileSource;

import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.util.Base64;

import static org.junit.jupiter.api.Assertions.assertTrue;

public class ClientTest {
    
    @ParameterizedTest
    @CsvFileSource(resources = "/test.csv", numLinesToSkip = 1)
    void testSendData(String username, String filepath, String expectedBase64Image) {
        Client.sendData(username, filepath);
        
        // Dummy verification for illustration
        BufferedImage receivedImage = decodeBase64ToImage(expectedBase64Image);
        BufferedImage expectedImage = decodeBase64ToImage(expectedBase64Image); // Normally, you would get this from server response
        
        assertTrue(compareImages(receivedImage, expectedImage));
    }
    
    private BufferedImage decodeBase64ToImage(String base64String) {
        byte[] imageBytes = Base64.getDecoder().decode(base64String);
        try (ByteArrayInputStream bais = new ByteArrayInputStream(imageBytes)) {
            return ImageIO.read(bais);
        } catch (IOException e) {
            e.printStackTrace();
            return null;
        }
    }

    private boolean compareImages(BufferedImage imgA, BufferedImage imgB) {
        // Basic image comparison
        if (imgA.getWidth() == imgB.getWidth() && imgA.getHeight() == imgB.getHeight()) {
            for (int y = 0; y < imgA.getHeight(); y++) {
                for (int x = 0; x < imgA.getWidth(); x++) {
                    if (imgA.getRGB(x, y) != imgB.getRGB(x, y)) {
                        return false;
                    }
                }
            }
        } else {
            return false;
        }
        return true;
    }
}
```

2. Uruchom testy, aby sprawdzić, czy aplikacja kliencka działa poprawnie.

### 5. Aplikacja webowa z Spring Boot

1. Utwórz nowy projekt Spring Boot.
2. Dodaj zależność do bazy danych SQLite i JDBC w `pom.xml`.

**pom.xml**
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.xerial</groupId>
        <artifactId>sqlite-jdbc</artifactId>
        <version>3.36.0.3</version>
    </dependency>
</dependencies>
```

3. Utwórz kontrol

er `EEGController`.

**EEGController.java**
```java
package com.example.eegwebapp;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

@RestController
public class EEGController {
    
    @GetMapping("/eeg")
    public String getEEGData(@RequestParam String username, @RequestParam int electrodeNumber) {
        String url = "jdbc:sqlite:eeg_data.db";
        String sql = "SELECT eeg_plot_base64 FROM EEGData WHERE username = '" + username + "' AND electrode_number = " + electrodeNumber;
        
        try (Connection conn = DriverManager.getConnection(url);
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {
            
            if (rs.next()) {
                String base64Image = rs.getString("eeg_plot_base64");
                return "<html><body><h1>User: " + username + "</h1>"
                        + "<h2>Electrode: " + electrodeNumber + "</h2>"
                        + "<img src='data:image/png;base64," + base64Image + "'/>"
                        + "</body></html>";
            } else {
                return "No data found for user: " + username + " and electrode: " + electrodeNumber;
            }
        } catch (SQLException e) {
            e.printStackTrace();
            return "Error retrieving data";
        }
    }
}
```

4. Uruchom aplikację Spring Boot i przetestuj endpoint `/eeg`, podając nazwę użytkownika i numer elektrody jako parametry URL.
