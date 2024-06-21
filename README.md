# Rozwiązania do powtórzeń oraz poradniki do kluczowych elementów które mogą się pojawić

## Przydatne linki

#### https://github.com/umcspro
#### https://github.com/kdmitruk
#### https://github.com/lukaszkurantdev
#### https://github.com/michaldziuba03/java
#### https://mvnrepository.com/

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

## Rozwiązanie kolokwium powtórzeniowego

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

3. Utwórz kontroler `EEGController`.

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

# Kolokwium 2021 Rysowanie Odcinków w JavaFX z Obsługą Sieci

## Opis Projektu

Ten projekt implementuje program w Java, który uruchamia serwer nasłuchujący na wybranym porcie oraz wyświetla okno do rysowania grafiki 2D przy użyciu JavaFX. Program pozwala na podłączenie wielu klientów, z których każdy może wysłać wiadomość dotyczącą koloru lub współrzędnych odcinka do narysowania.


## Krok 3: Implementacja Serwera

**Server.java**

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.concurrent.ConcurrentHashMap;

public class Server {
    private static final int PORT = 12345;
    private static ConcurrentHashMap<Socket, String> clientColors = new ConcurrentHashMap<>();

    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("Server is listening on port " + PORT);

            while (true) {
                Socket socket = serverSocket.accept();
                System.out.println("New client connected");

                new ClientHandler(socket, clientColors).start();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**ClientHandler.java**

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.Socket;
import java.util.concurrent.ConcurrentHashMap;

public class ClientHandler extends Thread {
    private Socket socket;
    private ConcurrentHashMap<Socket, String> clientColors;

    public ClientHandler(Socket socket, ConcurrentHashMap<Socket, String> clientColors) {
        this.socket = socket;
        this.clientColors = clientColors;
    }

    @Override
    public void run() {
        try (BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()))) {
            String inputLine;
            while ((inputLine = in.readLine()) != null) {
                if (inputLine.matches("^[0-9A-Fa-f]{6}$")) {
                    clientColors.put(socket, "#" + inputLine);
                } else if (inputLine.matches("^-?\\d+(\\.\\d+)?\\s-?\\d+(\\.\\d+)?\\s-?\\d+(\\.\\d+)?\\s-?\\d+(\\.\\d+)?$")) {
                    String[] parts = inputLine.split("\\s+");
                    double x1 = Double.parseDouble(parts[0]);
                    double y1 = Double.parseDouble(parts[1]);
                    double x2 = Double.parseDouble(parts[2]);
                    double y2 = Double.parseDouble(parts[3]);
                    String color = clientColors.getOrDefault(socket, "#000000");

                    // Dodajemy odcinek do rysowania w GUI
                    DrawingApp.addLine(x1, y1, x2, y2, color);
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Krok 4: Implementacja GUI w JavaFX

**DrawingApp.java**

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.canvas.Canvas;
import javafx.scene.canvas.GraphicsContext;
import javafx.scene.input.KeyEvent;
import javafx.scene.layout.Pane;
import javafx.scene.paint.Color;
import javafx.stage.Stage;

import java.util.ArrayList;
import java.util.List;

public class DrawingApp extends Application {
    private static final int WIDTH = 500;
    private static final int HEIGHT = 500;
    private static List<Line> lines = new ArrayList<>();
    private static Canvas canvas;
    private static double offsetX = 0;
    private static double offsetY = 0;

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        canvas = new Canvas(WIDTH, HEIGHT);
        Pane root = new Pane(canvas);
        Scene scene = new Scene(root);

        scene.setOnKeyPressed(this::handleKeyPress);

        primaryStage.setTitle("Drawing App");
        primaryStage.setScene(scene);
        primaryStage.show();

        draw();
    }

    private void handleKeyPress(KeyEvent event) {
        switch (event.getCode()) {
            case UP:
                offsetY += 10;
                break;
            case DOWN:
                offsetY -= 10;
                break;
            case LEFT:
                offsetX += 10;
                break;
            case RIGHT:
                offsetX -= 10;
                break;
            default:
                break;
        }
        draw();
    }

    public static void addLine(double x1, double y1, double x2, double y2, String color) {
        lines.add(new Line(x1, y1, x2, y2, color));
        draw();
    }

    private static void draw() {
        GraphicsContext gc = canvas.getGraphicsContext2D();
        gc.setFill(Color.WHITE);
        gc.fillRect(0, 0, WIDTH, HEIGHT);

        for (Line line : lines) {
            gc.setStroke(Color.web(line.color));
            gc.strokeLine(line.x1 - offsetX, line.y1 - offsetY, line.x2 - offsetX, line.y2 - offsetY);
        }
        gc.setFill(Color.BLACK);
        gc.fillText("Offset X: " + offsetX + ", Offset Y: " + offsetY, 10, 10);
    }

    private static class Line {
        double x1, y1, x2, y2;
        String color;

        Line(double x1, double y1, double x2, double y2, String color) {
            this.x1 = x1;
            this.y1 = y1;
            this.x2 = x2;
            this.y2 = y2;
            this.color = color;
        }
    }
}
```

## Krok 5: Uruchomienie Programu

1. Uruchom `Server` jako aplikację Java.
2. Uruchom `DrawingApp` jako aplikację JavaFX.
3. Użyj dowolnego klienta TCP (np. telnet, netcat) do wysyłania wiadomości do serwera. Możesz użyć poniższych komend w terminalu, aby połączyć się z serwerem i wysyłać wiadomości:
   - Ustaw kolor: `echo "FF0000" | nc localhost 12345`
   - Wyślij współrzędne odcinka: `echo "100 100 200 200" | nc localhost 12345`
